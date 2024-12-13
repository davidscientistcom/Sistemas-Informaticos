# Práctica: Configuración de Vagrant con Autenticación SSH Personalizada

## Introducción
En esta práctica, aprenderás a configurar una máquina virtual utilizando Vagrant y a establecer un sistema de autenticación SSH mediante claves personalizadas. Esta configuración se realizará tanto en Windows como en Linux.

El objetivo es automatizar el proceso de configuración para que la máquina virtual acepte una clave pública generada en el host y permita la conexión SSH sin contraseña.

## Requisitos previos
1. **Vagrant** instalado en el host.
2. **VirtualBox** como proveedor de máquinas virtuales.
3. Conocimientos básicos de comandos de terminal.

## Generación de claves SSH en el host

### En Windows:
1. Abre **PowerShell** y ejecuta:
   ```powershell
   ssh-keygen -t rsa -b 2048 -C "vagrant_vm_key" -f $HOME\.ssh\vagrant_vm_key
   ```
   - **`-t rsa`**: Especifica el tipo de clave RSA.
   - **`-b 2048`**: Define el tamaño de la clave.
   - **`-C`**: Agrega un comentario para identificar la clave.
   - **`-f`**: Especifica el archivo donde se guardará la clave.

2. Esto generará dos archivos:
   - **`vagrant_vm_key`**: Clave privada.
   - **`vagrant_vm_key.pub`**: Clave pública.

### En Linux:
1. Abre una terminal y ejecuta:
   ```bash
   ssh-keygen -t rsa -b 2048 -C "vagrant_vm_key" -f ~/.ssh/vagrant_vm_key
   ```
2. Confirma la generación de los archivos:
   ```bash
   ls ~/.ssh/vagrant_vm_key*
   ```


## Configuración del `Vagrantfile`
El siguiente script configura una máquina virtual con Vagrant y asegura que la clave SSH generada en el host se copie correctamente y se utilice para la autenticación.

### **`Vagrantfile` Explicado Paso a Paso**
```ruby
Vagrant.configure("2") do |config|
  # Especifica la caja base de Ubuntu
  config.vm.box = "ubuntu/bionic64"

  # Configura una red privada con una IP fija
  config.vm.network "private_network", ip: "192.168.33.10"

  # Configura los recursos de hardware
  config.vm.provider "virtualbox" do |vb|
    vb.memory = "4096"  # 4 GB de memoria
    vb.cpus = 2          # 2 CPUs
  end

  # Copia la clave pública generada en el host a la VM
  config.vm.provision "file", source: "#{Dir.home}/.ssh/vagrant_vm_key.pub", destination: "/home/vagrant/.ssh/vagrant_vm_key.pub"

  # Configuración inicial del servidor SSH
  config.vm.provision "shell", run: "once" do |s|
    s.inline = <<-SHELL
      # Actualizar e instalar OpenSSH Server
      apt-get update
      apt-get install -y openssh-server

      # Configurar el servidor SSH para aceptar claves públicas
      sed -i 's/#PubkeyAuthentication yes/PubkeyAuthentication yes/' /etc/ssh/sshd_config
      sed -i 's/#PasswordAuthentication yes/PasswordAuthentication no/' /etc/ssh/sshd_config

      # Crear el directorio .ssh si no existe
      mkdir -p /home/vagrant/.ssh
      chmod 700 /home/vagrant/.ssh
      chown vagrant:vagrant /home/vagrant/.ssh

      # Sobrescribir el contenido de authorized_keys con la clave pública
      cat /home/vagrant/.ssh/vagrant_vm_key.pub > /home/vagrant/.ssh/authorized_keys

      # Ajustar permisos
      chmod 600 /home/vagrant/.ssh/authorized_keys
      chown vagrant:vagrant /home/vagrant/.ssh/authorized_keys

      # Reiniciar el servidor SSH
      systemctl restart sshd
    SHELL
  end

  # Configuración para evitar prompts de SSH
  config.ssh.insert_key = false
  config.ssh.extra_args = ["-o StrictHostKeyChecking=no", "-o UserKnownHostsFile=/dev/null"]
end
```

### **Explicación del Script**
1. **Copia de la clave pública**:
   - Utiliza `config.vm.provision "file"` para copiar la clave pública desde el host al archivo `/home/vagrant/.ssh/vagrant_vm_key.pub` dentro de la VM.

2. **Configuración del servidor SSH**:
   - Se asegura de que el servidor SSH esté instalado y configurado para aceptar claves públicas (habilitando `PubkeyAuthentication`).
   - La clave pública se sobrescribe en el archivo `authorized_keys` para permitir la autenticación.
   - Ajusta los permisos del directorio `.ssh` y el archivo `authorized_keys`.

3. **Evitar prompts de SSH**:
   - La opción `config.ssh.insert_key = false` desactiva la generación automática de claves por parte de Vagrant.
   - `config.ssh.extra_args` desactiva verificaciones estrictas del archivo `known_hosts`.



## Comandos Importantes

### **1. Remover entradas conflictivas en `known_hosts`**
Si la máquina ya ha sido recreada y el host guarda una clave antigua, podría aparecer un error de autenticación. Usa este comando para eliminar la entrada antigua:

```bash
ssh-keygen -R [127.0.0.1]:2222
```

- **127.0.0.1:2222**: Indica la IP y el puerto de la máquina virtual en el host.
- Este comando elimina la clave asociada a esa IP/puerto en el archivo `known_hosts`.

### **2. Probar la conexión SSH manualmente**
Desde el host, prueba conectarte a la máquina virtual con:

```bash
ssh -i ~/.ssh/vagrant_vm_key -p 2222 vagrant@127.0.0.1
```

- **`-i ~/.ssh/vagrant_vm_key`**: Especifica la clave privada.
- **`-p 2222`**: Indica el puerto configurado por Vagrant.

Si todo está configurado correctamente, deberías conectarte sin contraseña.


## Resolución de Problemas

### **1. La clave no se copia a la VM**
- Verifica que la ruta de la clave en el `Vagrantfile` sea correcta:
  ```ruby
  source: "#{Dir.home}/.ssh/vagrant_vm_key.pub"
  ```
- Usa una ruta absoluta si es necesario:
  ```ruby
  source: "C:/Users/tu_usuario/.ssh/vagrant_vm_key.pub"
  ```

### **2. Error de permisos en `authorized_keys`**
Si el archivo `authorized_keys` no tiene permisos correctos, configura manualmente desde la VM:

```bash
chmod 600 /home/vagrant/.ssh/authorized_keys
chmod 700 /home/vagrant/.ssh
chown -R vagrant:vagrant /home/vagrant/.ssh
```

### **3. SSH no permite la conexión**
- Asegúrate de que el servidor SSH esté corriendo:
  ```bash
  sudo systemctl restart sshd
  ```
- Revisa los logs de SSH para más detalles:
  ```bash
  sudo journalctl -u ssh
  ```