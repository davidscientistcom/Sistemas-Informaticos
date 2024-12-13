### **Práctica 2: Personalizar la box con tu clave SSH**

El objetivo de esta práctica es usar la **box base** creada en la práctica anterior para iniciar una máquina virtual con Vagrant, configurarle una clave SSH personalizada y verificar que puedes conectarte a ella.



#### **Paso 1: Añadir la box a Vagrant**
1. Añade la box creada en la Práctica 1:
   ```bash
   vagrant box add ubuntu-server ./ubuntu-server.box
   ```

2. Crea un nuevo directorio de proyecto:
   ```bash
   mkdir vagrant_project && cd vagrant_project
   vagrant init ubuntu-server
   ```

3. Edita el `Vagrantfile` para añadir una red privada:
   ```ruby
   Vagrant.configure("2") do |config|
     config.vm.box = "ubuntu-server"
     config.vm.network "private_network", type: "dhcp"
   end
   ```

4. Inicia la máquina:
   ```bash
   vagrant up
   ```



#### **Paso 2: Generar una clave SSH personalizada**
1. **Desde tu máquina anfitriona**, genera una clave SSH:
   ```bash
   ssh-keygen -t rsa -b 4096 -C "clave personalizada" -f ~/.ssh/id_rsa_custom -N ""
   ```
   Esto genera:
   - Clave privada: `~/.ssh/id_rsa_custom`.
   - Clave pública: `~/.ssh/id_rsa_custom.pub`.



#### **Paso 3: Copiar la clave pública a la máquina virtual**
1. Verifica la IP de la máquina:
   ```bash
   vagrant ssh-config
   ```
   Busca el valor de `HostName` para conocer la IP asignada por DHCP.

2. Usa `scp` para copiar la clave pública:
   ```bash
   scp -i ~/.vagrant.d/insecure_private_key ~/.ssh/id_rsa_custom.pub vagrant@<IP_VM>:/tmp/id_rsa_custom.pub
   ```



#### **Paso 4: Añadir la clave pública dentro de la máquina virtual**
1. Conéctate a la máquina virtual:
   ```bash
   vagrant ssh
   ```

2. Copia la clave pública al archivo `authorized_keys` del usuario `vagrant`:
   ```bash
   cat /tmp/id_rsa_custom.pub >> ~/.ssh/authorized_keys
   chmod 600 ~/.ssh/authorized_keys
   rm /tmp/id_rsa_custom.pub
   ```



#### **Paso 5: Probar el acceso con la clave personalizada**
Desde tu máquina anfitriona, prueba conectarte a la máquina con la nueva clave privada:
```bash
ssh -i ~/.ssh/id_rsa_custom vagrant@<IP_VM>
```
