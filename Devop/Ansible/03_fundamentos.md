### 3. **Fundamentos de Ansible**

En esta sección, exploraremos los conceptos fundamentales de Ansible. Aunque no entraremos en detalles avanzados, construiremos una base sólida para trabajar con esta herramienta.



#### 3.1. Componentes principales de Ansible

1. **Controlador y nodos gestionados**:
   - El **nodo controlador** es la máquina donde instalas Ansible. Desde aquí, emites los comandos y ejecutas los playbooks.
   - Los **nodos gestionados** son las máquinas que Ansible controla mediante SSH. No requieren instalación de software adicional, solo Python.

   **Ejemplo práctico:**
   Si estás trabajando desde tu máquina local, esta será el nodo controlador, y tus máquinas virtuales serán los nodos gestionados.



2. **Playbooks**:
   - Son archivos YAML donde defines tareas a ejecutar en los nodos gestionados.
   - Cada playbook describe **qué hacer** y **dónde hacerlo**.

   **Ejemplo de playbook básico (`ejemplo.yml`):**

   ```yaml
   
   - name: Instalar paquetes básicos
     hosts: all
     tasks:
       - name: Actualizar lista de paquetes
         apt:
           update_cache: yes
   ```

   - Ejecuta este playbook con:
     ```bash
     ansible-playbook -i inventario ejemplo.yml
     ```



3. **Inventarios**:
   - Un inventario es una lista de nodos gestionados, organizada en grupos. Puede ser un archivo estático (como el que ya configuraste) o dinámico.
   - Ejemplo de inventario estático:

     ```ini
     [webservers]
     192.168.56.10 ansible_user=vagrant ansible_ssh_private_key_file=~/.ssh/vagrant_vm_key
     ```



4. **Módulos**:
   - Los módulos son las herramientas que Ansible usa para realizar tareas específicas. Ejemplo: instalar paquetes, gestionar usuarios o copiar archivos.
   - Puedes usar módulos directamente en comandos ad-hoc o dentro de playbooks.

   **Ejemplo de módulo ad-hoc:**
   ```bash
   ansible all -i inventario -m apt -a "name=curl state=latest"
   ```



5. **Roles**:
   - Los roles son una forma de organizar tareas, variables y archivos relacionados. Se utilizan en proyectos más grandes para mantener el orden.



6. **Variables**:
   - Las variables permiten personalizar playbooks y roles. Se definen en archivos YAML o directamente en los playbooks.
   - Ejemplo:

     ```yaml
     vars:
       paquete: curl

     tasks:
       - name: Instalar paquete
         apt:
           name: "{{ paquete }}"
           state: latest
     ```



#### 3.2. Conceptos adicionales

1. **Conexión SSH**:
   - Ansible utiliza SSH para conectarse a los nodos gestionados. Es fundamental que las claves SSH estén correctamente configuradas.

2. **Ejecución ad-hoc**:
   - Es una forma rápida de ejecutar comandos en los nodos sin usar un playbook.
   - Ejemplo:

     ```bash
     ansible all -i inventario -m ping
     ```



#### 3.3. ¿Qué se puede hacer con Ansible?

1. **Gestión de configuraciones**:
   - Configurar servidores con ajustes específicos, como instalar Apache, configurar firewall, etc.

2. **Despliegue de aplicaciones**:
   - Automatizar la instalación y configuración de aplicaciones.

3. **Orquestación de tareas**:
   - Coordinar procesos entre múltiples servidores, como un despliegue continuo.



#### 3.4. Primeros pasos prácticos

**Ejercicio: Crear un playbook básico**

1. Crea un archivo llamado `actualizar_sistema.yml` con el siguiente contenido:

   ```yaml
   
   - name: Actualizar paquetes en los servidores
     hosts: all
     tasks:
       - name: Actualizar lista de paquetes
         apt:
           update_cache: yes
       - name: Actualizar todos los paquetes instalados
         apt:
           upgrade: dist
   ```

2. Ejecuta el playbook:

   ```bash
   ansible-playbook -i inventario actualizar_sistema.yml
   ```

3. Verifica que los paquetes han sido actualizados en los nodos gestionados.


#### 3.3. Módulos más importantes.
### **Módulos más importantes de Ansible**

En esta sección, exploraremos los módulos más utilizados en Ansible para tareas comunes, como gestión de paquetes, servicios, archivos y usuarios. Incluiremos ejemplos prácticos que los alumnos puedan ejecutar.

Recordad que si queréis especificar un fichero de inventario, tenéis que hacer algo como:

```bash 
ansible -i inventario all -m ping
```

Además, hay veces, en las que tendremos que usar el root, para poder usar  `apt` (`root`)

### Solución: Elevar privilegios con `become`

En Ansible, puedes usar la opción `become` para ejecutar comandos como superusuario (`sudo`).

#### 1. **Comando ad-hoc con `--become`**

Incluye la opción `--become` en tu comando para ejecutar el módulo `apt` con privilegios de superusuario:

```bash
ansible -i inventario all -m apt -a "update_cache=yes" --become
```

Si tu usuario requiere contraseña para `sudo`, puedes añadir `--ask-become-pass` para que Ansible la solicite:

```bash
ansible -i inventario all -m apt -a "update_cache=yes" --become --ask-become-pass
```

---

#### 2. **Configurar `become` en el playbook**

Si estás trabajando con un playbook, puedes especificar `become: yes` a nivel de tareas o de todo el playbook. Ejemplo:

**Playbook: `actualizar_sistema.yml`**

```yaml
---
- name: Actualizar sistema
  hosts: all
  become: yes
  tasks:
    - name: Actualizar lista de paquetes
      apt:
        update_cache: yes

    - name: Actualizar todos los paquetes
      apt:
        upgrade: dist
```

Ejecuta el playbook como siempre:

```bash
ansible-playbook -i inventario actualizar_sistema.yml
```

### Configuración permanente de `become` en Ansible

Si necesitas que todos los comandos se ejecuten con `sudo` de forma predeterminada, puedes configurarlo en el archivo `ansible.cfg`:

```ini
[defaults]
become = True
```

Esto asegura que Ansible intente elevar privilegios automáticamente.


#### 1. **Módulo `ping`**
El módulo más básico para probar la conectividad entre el controlador y los nodos gestionados.

**Uso:**
```bash
ansible all -m ping
```

**Salida esperada:**
```json
192.168.56.10 | SUCCESS => {
    "changed": false,
    "ping": "pong"
}
```


#### 2. **Módulo `apt`**
Gestiona paquetes en sistemas basados en Debian.

**Ejemplo 1: Actualizar la lista de paquetes**
```bash
ansible all -m apt -a "update_cache=yes"
```

**Ejemplo 2: Instalar un paquete**
```bash
ansible all -m apt -a "name=curl state=present"
```

**Ejemplo 3: Desinstalar un paquete**
```bash
ansible all -m apt -a "name=curl state=absent"
```

**Ejemplo 4: Actualización del sistema**
```bash
ansible all -m apt -a "upgrade=dist"
```


#### 3. **Módulo `service`**
Gestiona servicios en los nodos.

**Ejemplo 1: Iniciar un servicio**
```bash
ansible all -m service -a "name=ssh state=started"
```

**Ejemplo 2: Detener un servicio**
```bash
ansible all -m service -a "name=ssh state=stopped"
```

**Ejemplo 3: Reiniciar un servicio**
```bash
ansible all -m service -a "name=ssh state=restarted"
```


#### 4. **Módulo `copy`**
Copia archivos desde el nodo controlador a los nodos gestionados.

**Ejemplo: Copiar un archivo**
```bash
ansible all -m copy -a "src=/path/local/archivo.txt dest=/tmp/archivo.txt"
```

#### 5. **Módulo `file`**
Gestiona permisos y directorios.

**Ejemplo 1: Crear un directorio**
```bash
ansible all -m file -a "path=/tmp/nuevo_directorio state=directory mode=0755"
```

**Ejemplo 2: Cambiar permisos de un archivo**
```bash
ansible all -m file -a "path=/tmp/archivo.txt mode=0644"
```

**Ejemplo 3: Eliminar un archivo o directorio**
```bash
ansible all -m file -a "path=/tmp/archivo.txt state=absent"
```

#### 6. **Módulo `user`**
Gestiona usuarios en los nodos.

**Ejemplo 1: Crear un usuario**
```bash
ansible all -m user -a "name=nuevo_usuario state=present"
```

**Ejemplo 2: Eliminar un usuario**
```bash
ansible all -m user -a "name=nuevo_usuario state=absent"
```

**Ejemplo 3: Añadir un usuario a un grupo**
```bash
ansible all -m user -a "name=nuevo_usuario groups=admin append=yes"
```

#### 7. **Módulo `command`**
Ejecuta comandos en los nodos.

**Ejemplo: Listar el contenido de un directorio**
```bash
ansible all -m command -a "ls -la /tmp"
```


#### 8. **Módulo `shell`**
Ejecuta comandos usando el shell del sistema.

**Ejemplo: Crear un archivo con un contenido**
```bash
ansible all -m shell -a "echo 'Hola Ansible' > /tmp/saludo.txt"
```

#### 9. **Módulo `debug`**
Muestra mensajes en el proceso de ejecución.

**Ejemplo: Mostrar un mensaje personalizado**
```yaml
---
- name: Prueba del módulo debug
  hosts: all
  tasks:
    - debug:
        msg: "Este es un mensaje de prueba desde {{ inventory_hostname }}"
```

Ejecuta el playbook con:
```bash
ansible-playbook -i inventario debug_playbook.yml
```


#### 10. **Módulo `git`**
Gestiona repositorios Git en los nodos.

**Ejemplo: Clonar un repositorio**
```bash
ansible all -m git -a "repo=https://github.com/tu-repositorio.git dest=/tmp/mi_repositorio"
```


### Ejercicio práctico
**Tarea: Instalar y configurar Apache en los nodos**

1. Crea un playbook llamado `configurar_apache.yml` con el siguiente contenido:

```yaml
---
- name: Instalar y configurar Apache
  hosts: all
  tasks:
    - name: Instalar Apache
      apt:
        name: apache2
        state: present
        update_cache: yes

    - name: Iniciar el servicio Apache
      service:
        name: apache2
        state: started
        enabled: yes

    - name: Crear un archivo de bienvenida
      copy:
        content: "¡Bienvenido a mi servidor gestionado con Ansible!"
        dest: /var/www/html/index.html
```

2. Ejecuta el playbook:

```bash
ansible-playbook -i inventario configurar_apache.yml
```

3. Verifica accediendo a `http://192.168.56.10` desde tu navegador.


#### 3.4. Comandos especificando el host.
Si deseas crear un usuario en una máquina específica en lugar de en todos los nodos del inventario, puedes hacerlo de varias formas. Aquí te explico cómo:



### 1. **Comando ad-hoc especificando el host**
Utiliza el nombre o la IP del host directamente en el comando.

**Ejemplo: Crear un usuario llamado `nuevo_usuario` en `192.168.56.10`:**

```bash
ansible 192.168.56.10 -i inventario -m user -a "name=nuevo_usuario state=present" --become
```



### 2. **Playbook para un único host**
Define el host específico en el playbook en lugar de aplicar los cambios a todos los nodos.

**Nota**:El argumento -i inventario es necesario porque Ansible siempre requiere un archivo de inventario para localizar información sobre los hosts y configuraciones adicionales, incluso si especificas la IP directamente. A continuación, te explico por qué sucede esto y cómo funciona.

**Ejemplo de playbook (`crear_usuario.yml`):**

```yaml
---
- name: Crear un usuario en un host específico
  hosts: 192.168.56.10
  become: yes
  tasks:
    - name: Crear usuario nuevo_usuario
      user:
        name: nuevo_usuario
        state: present
        shell: /bin/bash
```

Ejecuta el playbook con:

```bash
ansible-playbook -i inventario crear_usuario.yml
```



### 3. **Usar grupos o etiquetas en el inventario**
Si planeas repetir este proceso en máquinas específicas con frecuencia, puedes usar grupos en el archivo de inventario.

**Ejemplo de inventario:**

```ini
[servidor_especial]
192.168.56.10 ansible_user=vagrant ansible_ssh_private_key_file=~/.ssh/vagrant_vm_key
```

En el playbook, apunta a ese grupo específico:

```yaml
---
- name: Crear usuario en servidor especial
  hosts: servidor_especial
  become: yes
  tasks:
    - name: Crear usuario nuevo_usuario
      user:
        name: nuevo_usuario
        state: present
        shell: /bin/bash
```

Ejecuta el playbook con:

```bash
ansible-playbook -i inventario crear_usuario.yml
```

---

### 4. **Filtrar hosts en tiempo de ejecución**
Usa la opción `--limit` para restringir la ejecución de un playbook a un nodo específico.

**Ejemplo:**

```bash
ansible-playbook -i inventario crear_usuario.yml --limit 192.168.56.10
```


### Recomendaciones
- **Verifica los permisos**: Si el usuario que usas para conectarte no tiene privilegios administrativos, recuerda añadir `--become` para usar `sudo`.
- **Mantén el orden en el inventario**: Organiza los hosts en grupos si necesitas realizar acciones específicas con regularidad.
