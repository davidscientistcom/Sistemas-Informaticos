# **Uso de Certificados Digitales en SSH con Vagrant y Ansible**


## **1. ¿Por Qué Utilizamos Certificados Digitales en SSH?**

### **1.1. Objetivo Principal**

En estas prácticas, utilizamos **certificados digitales** para:
- **Asegurar la autenticación SSH** entre el host (tu máquina) y las máquinas virtuales (VMs) creadas con Vagrant.
- **Eliminar la necesidad de contraseñas**, permitiendo conexiones seguras y automáticas.
- **Automatizar la configuración SSH** usando Ansible, garantizando consistencia y seguridad.



### **1.2. ¿Por Qué No Usamos Contraseñas?**
1. **Seguridad Mejorada:**
   - Las contraseñas son vulnerables a ataques de fuerza bruta.
   - Los certificados digitales utilizan **cifrado asimétrico**, lo cual es mucho más seguro.

2. **Automatización sin Interrupciones:**
   - Al utilizar claves SSH, se permite la **conexión sin interacción** (sin necesidad de ingresar contraseñas), lo cual es ideal para scripts y despliegues automáticos con Ansible.

3. **Consistencia y Portabilidad:**
   - Al definir las claves en el `Vagrantfile` y en los playbooks de Ansible, todos los entornos son **idénticos**, sin diferencias debidas a contraseñas individuales.



## **2. ¿Cómo Funciona la Autenticación SSH con Certificados Digitales?**

### **2.1. Concepto General**

La **autenticación SSH con certificados digitales** utiliza un **par de claves**:
- **Clave Privada (`id_rsa`)**:
  - Se guarda en el host.
  - **Nunca** se comparte. Es **secreta** y **personal**.
- **Clave Pública (`id_rsa.pub`)**:
  - Se copia a las VMs.
  - Es **visible** y **segura** para compartir.
  - Se almacena en el archivo `authorized_keys` de la VM.

### **2.2. Proceso de Autenticación**

1. **Inicio de Conexión:**
   - El host inicia una conexión SSH a la VM usando el comando:
     ```bash
     ssh -i ~/.ssh/vagrant_vm_key vagrant@192.168.33.10
     ```
     - `-i` especifica el archivo de clave privada a utilizar.
     - `vagrant` es el usuario en la VM.
     - `192.168.33.10` es la IP de la VM (red privada).

2. **Intercambio de Claves:**
   - La VM **compara** la clave pública en su archivo `authorized_keys` con la clave privada del host.
   - Si coinciden, se establece una **conexión cifrada** sin necesidad de contraseña.

3. **Autenticación Exitosa:**
   - Si la clave coincide, el usuario puede acceder a la VM.
   - Si no coincide, la conexión se rechaza.



## **3. Ficheros Clave en el Proceso de Autenticación**

### **3.1. `id_rsa` (Clave Privada) en el Host**

- **Ubicación:** 
  ```sh
  ~/.ssh/vagrant_vm_key
  ```

- **Contenido:**
  ```plaintext
  --BEGIN RSA PRIVATE KEY--
  (Contenido cifrado)
  --END RSA PRIVATE KEY--
  ```

- **Función:**
  - Es la **clave secreta** utilizada para **firmar** la solicitud de conexión.
  - Se mantiene **privada** en el host y nunca se comparte.
  - **Permisos:** Solo el propietario puede leerla (`chmod 600`).



### **3.2. `id_rsa.pub` (Clave Pública) en el Host**

- **Ubicación:**
  ```sh
  ~/.ssh/vagrant_vm_key.pub
  ```

- **Contenido:**
  ```plaintext
  ssh-rsa AAAAB3... usuario@host
  ```

- **Función:**
  - Es la **clave pública** que se comparte con las VMs.
  - Se coloca en el archivo `authorized_keys` de cada VM.
  - **Seguridad:** Es seguro compartirla, ya que **no se puede derivar** la clave privada a partir de ella.



### **3.3. `authorized_keys` en la VM**

- **Ubicación en la VM:**
  ```sh
  /home/vagrant/.ssh/authorized_keys
  ```

- **Contenido:**
  ```plaintext
  ssh-rsa AAAAB3... usuario@host
  ```

- **Función:**
  - Almacena **todas las claves públicas** autorizadas para conectarse a la VM.
  - Si una clave pública coincide con la clave privada del host, se **permite la conexión**.

- **Permisos:**
  ```sh
  chmod 600 /home/vagrant/.ssh/authorized_keys
  chown vagrant:vagrant /home/vagrant/.ssh/authorized_keys
  ```



### **3.4. `known_hosts` en el Host**

- **Ubicación en el Host:**
  ```sh
  ~/.ssh/known_hosts
  ```

- **Función:**
  - Almacena la **huella digital (fingerprint)** de las máquinas a las que el host se ha conectado previamente.
  - Previene **ataques de Man-in-the-Middle (MitM)** al verificar que la VM no ha cambiado su clave de host.

- **Contenido:**
  ```plaintext
  192.168.33.10 ssh-rsa AAAAB3... (fingerprint)
  ```

- **Importante:**
  - Si se destruye y recrea una VM, su clave de host cambiará, provocando el error:
    ```plaintext
    WARNING: REMOTE HOST IDENTIFICATION HAS CHANGED!
    ```
  - Se soluciona con:
    ```sh
    ssh-keygen -R 192.168.33.10
    ```



## **4. Automatización con Vagrant y Ansible**

### **4.1. Por Qué No Copiamos las Claves Manualmente**

1. **Consistencia y Repetibilidad:**
   - Ansible **automatiza** la copia de claves públicas a las VMs, garantizando consistencia en cada despliegue.

2. **Seguridad:**
   - Se evita el **error humano** y la exposición innecesaria de las claves privadas.

3. **Escalabilidad:**
   - Si se crean múltiples VMs, Ansible asegura que todas tengan la misma configuración SSH sin intervención manual.



### **4.2. Proceso en el `Vagrantfile`**

```ruby
config.vm.provision "file", run: "once", source: "#{Dir.home}/.ssh/vagrant_vm_key.pub", destination: "/home/vagrant/.ssh/vagrant_vm_key.pub"
```

- **Copia la clave pública** desde el host a la VM.
- Se usa `run: "once"` para evitar copiarla en cada `vagrant up`.



### **4.3. Configuración con Ansible**

Ansible se encarga de:
- **Mover la clave pública** al archivo `authorized_keys`.
- **Configurar permisos** para garantizar seguridad.
- **Configurar el servidor SSH** para aceptar autenticación mediante claves públicas.

```yaml
- name: Configurar SSH para autenticación con clave pública
  hosts: all
  become: yes
  tasks:
    - name: Copiar clave pública
      copy:
        src: /home/vagrant/.ssh/vagrant_vm_key.pub
        dest: /home/vagrant/.ssh/authorized_keys
        owner: vagrant
        group: vagrant
        mode: 0600

    - name: Configurar SSHD para aceptar autenticación con clave pública
      lineinfile:
        path: /etc/ssh/sshd_config
        regexp: '^#?PubkeyAuthentication'
        line: 'PubkeyAuthentication yes'
      notify: Reiniciar SSH

  handlers:
    - name: Reiniciar SSH
      service:
        name: ssh
        state: restarted
```



## **5. Casos de Uso Comunes y Beneficios**

### **5.1. Despliegue Continuo (CI/CD)**
- **Automatiza despliegues sin intervención humana.**
- Las claves SSH aseguran que **solo** los hosts autorizados pueden desplegar código.

### **5.2. Gestión Remota de Servidores**
- Administradores pueden acceder a múltiples servidores sin tener que recordar contraseñas.

### **5.3. Escenarios DevOps Multi-VM**
- Al usar Vagrant y Ansible, es posible crear redes privadas de VMs con autenticación SSH centralizada, ideal para **pruebas de integración** o **simulación de entornos de producción**.

