### 6. Provisionamiento Inicial de la Máquina Virtual

Aunque la provisión completa de la máquina virtual se abordará con **Ansible** en capítulos posteriores, en esta sección configuraremos un **provisioning básico** para realizar el setup inicial de SSH. Esto permitirá acceder a la máquina desde Windows y preparar el entorno para la administración mediante Ansible.

#### 6.1 Introducción al Provisionamiento en Vagrant

En Vagrant, el **provisioning** es el proceso de configuración de una máquina virtual después de su creación. Vagrant permite ejecutar scripts de shell y otros métodos de provisión en la máquina virtual durante su creación o cada vez que la máquina se inicia. Sin embargo, para evitar que el script de configuración de SSH se ejecute cada vez, configuraremos el script para que se ejecute una sola vez, utilizando la opción **`run: "once"`**.

#### 6.2 Configuración de Provisioning "Once" para SSH

El objetivo de este script es preparar la máquina para que sea accesible mediante **SSH** desde un sistema Windows. Esto es particularmente útil para entornos donde se requiere administrar la máquina a través de Ansible, ya que nos aseguramos de que el acceso SSH esté correctamente configurado desde el principio.

##### Ejemplo de Vagrantfile con Provisión "Once" para Configuración de SSH

Aquí tienes un ejemplo de un Vagrantfile que incluye un bloque de provisión "once" para configurar SSH.

```ruby
Vagrant.configure("2") do |config|
  config.vm.box = "ubuntu/bionic64"
  
  # Configuración de red privada
  config.vm.network "private_network", ip: "192.168.33.10"
  
  # Configuración de recursos de hardware
  config.vm.provider "virtualbox" do |vb|
    vb.memory = "2048"
    vb.cpus = 2
  end

  # Provisión para configurar SSH al iniciarse la máquina (solo se ejecuta una vez)
  config.vm.provision "shell", run: "once" do |s|
    s.inline = <<-SHELL
      # Actualizar e instalar el servidor SSH
      apt-get update
      apt-get install -y openssh-server

      # Configurar el servidor SSH para aceptar conexiones externas
      sed -i 's/#Port 22/Port 22/' /etc/ssh/sshd_config
      sed -i 's/#PasswordAuthentication yes/PasswordAuthentication yes/' /etc/ssh/sshd_config
      systemctl restart sshd
    SHELL
  end
end
```

En este Vagrantfile:

1. Se instala **OpenSSH Server** y se configuran los ajustes necesarios para aceptar conexiones SSH externas.
2. El comando `sed` se utiliza para editar el archivo de configuración `sshd_config`:
   - Se habilita el puerto 22.
   - Se habilita la autenticación por contraseña.
3. **`systemctl restart sshd`** reinicia el servicio SSH para que los cambios surtan efecto.

Este script se ejecutará **una sola vez** durante la primera creación de la máquina, gracias a la opción `run: "once"`, lo que evita que se aplique en reinicios futuros.

#### 6.3 Validación de la Conexión SSH desde Windows

Una vez que hayas ejecutado el comando `vagrant up` y la máquina virtual esté funcionando, puedes validar la configuración de SSH desde un sistema Windows. Aquí tienes dos métodos comunes para conectarte a la máquina virtual desde Windows:

##### Método 1: Usando PuTTY

1. **Descarga e instala PuTTY** desde [https://www.putty.org](https://www.putty.org).
2. Abre PuTTY y configura la conexión:
   - En "Host Name (or IP address)", introduce la IP configurada en el Vagrantfile (en este caso, `192.168.33.10`).
   - En "Port", ingresa `22`.
3. Haz clic en "Open" para iniciar la sesión SSH.
4. Cuando se te pida el usuario, ingresa `vagrant`, que es el usuario predeterminado de Vagrant. La contraseña también es `vagrant`.

##### Método 2: Usando PowerShell

1. Abre PowerShell en Windows.
2. Ingresa el siguiente comando SSH para conectarte a la máquina virtual:
   
   ```bash
   ssh vagrant@192.168.33.10
   ```

3. Cuando se te pida la contraseña, ingresa `vagrant`.

> **Nota**: Este paso es fundamental para confirmar que la máquina virtual está lista para ser gestionada por Ansible desde un sistema Windows.

#### 6.4 Verificación de la Ejecución del Script de Provisioning

Para verificar que el script de provisión "once" se ejecutó correctamente:

1. **Accede a la máquina virtual** mediante `vagrant ssh`.
2. Comprueba el estado del servicio SSH con el siguiente comando:
   
   ```bash
   systemctl status sshd
   ```

   Este comando debería mostrar que el servicio SSH está activo y en ejecución.

3. **Revisa el archivo de configuración** `/etc/ssh/sshd_config` para confirmar que los cambios se aplicaron correctamente, en especial las líneas que habilitan el puerto 22 y la autenticación por contraseña.
