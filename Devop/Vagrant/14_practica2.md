## Instalación de la máquina Virtual
Ojo, para crear una box, no es tan directo como crear la máquina virtual de cero. Tenemos que añadir una serie de cosas para que funcione.


## 1. Preparar el entorno en VirtualBox

1. **Crear la máquina virtual** en VirtualBox (desde la ISO de Ubuntu Server).
   - Tal y como hemos aprendido


2. **Arrancar Ubuntu** y acceder con el usuario que creaste durante la instalación (Lo recomendable es cuando lo creamos poner vagrant con pass vagrant por compatibilidad)
3. **Actualizar paquetes** (dentro de la VM):
   ```bash
   sudo apt-get update
   sudo apt-get upgrade -y
   ```


## 2. Instalar VirtualBox Guest Additions

Es indispensable para que Vagrant pueda configurar la red y sincronizar carpetas. Para instalar las Guest Additions:

1. **Instalar las dependencias** necesarias para compilar:
   ```bash
   sudo apt-get install -y build-essential dkms linux-headers-$(uname -r)
   ```
2. **Insertar la imagen de Guest Additions** (desde la ventana de la VM en VirtualBox: *Dispositivos* → *Insertar imagen de CD de las “Guest Additions”*).
3. **Montar y ejecutar el instalador** dentro de la VM:
   ```bash
   sudo mkdir /mnt/cdrom
   sudo mount /dev/cdrom /mnt/cdrom
   sudo /mnt/cdrom/VBoxLinuxAdditions.run
   sudo umount /mnt/cdrom
   ```
   - Asegúrate de que no se hayan reportado errores en la instalación.

4. **Reinicia** la máquina para que los cambios entren en vigor:
   ```bash
   sudo reboot
   ```


## 3. Crear y configurar el usuario `vagrant`

Aunque puedes usar el usuario que creaste al instalar, se recomienda **crear un usuario “vagrant”** para que Vagrant funcione de manera estándar. 

1. **Crear el usuario**:
   ```bash
   sudo adduser vagrant
   ```
   - Asigna cualquier contraseña temporal (aunque luego no la usarás).


2. **Conceder privilegios `sudo` sin contraseña**:
   ```bash
   sudo usermod -aG sudo vagrant
   # Crear un archivo de sudoers específico:
   echo "vagrant ALL=(ALL) NOPASSWD:ALL" | sudo tee /etc/sudoers.d/99_vagrant
   sudo chmod 440 /etc/sudoers.d/99_vagrant
   ```
3. **Añadir la clave “insegura” de Vagrant**. Esta es la clave pública que Vagrant usa por defecto para la conexión SSH inicial:
   ```bash
   sudo mkdir /home/vagrant/.ssh
   sudo chmod 700 /home/vagrant/.ssh
   sudo chown vagrant:vagrant /home/vagrant/.ssh

   # Descargar la clave pública oficial "insegura" de Vagrant
   cd /home/vagrant/.ssh
   sudo curl -L https://raw.githubusercontent.com/hashicorp/vagrant/main/keys/vagrant.pub -o authorized_keys

   # Ajustar permisos y propietario
   sudo chmod 600 /home/vagrant/.ssh/authorized_keys
   sudo chown vagrant:vagrant /home/vagrant/.ssh/authorized_keys
   ```

4. **Opcional**: Puedes eliminar el usuario que creaste durante la instalación inicial (si no lo deseas). Pero normalmente no hace daño dejarlo.



## 4. Ajustes finales y limpieza

Antes de empaquetar la VM, conviene dejarla lo más limpia posible.

1. **Eliminar temporales**:
   ```bash
   sudo apt-get autoremove -y
   sudo apt-get clean
   ```
2. **Historial de bash** (opcional, por seguridad):
   ```bash
   history -c
   cat /dev/null > ~/.bash_history
   ```
3. **Apagar la máquina**:
   ```bash
   sudo shutdown -h now
   ```



## 5. Empaquetar la máquina como box de Vagrant

En tu **host** (donde tienes VirtualBox instalado), vas a empaquetar la VM recién creada. Supongamos que en VirtualBox la VM se llama “ubuntu_server_2410”:

1. Abre una terminal **en tu máquina host**.
2. Ejecuta:
   ```bash
   vagrant package --base ubuntu_server_2410 --output ubuntu_server_2410.box
   ```
   - `--base` es el nombre exacto de la VM en VirtualBox.
   - `--output` especifica el nombre del archivo .box resultante.

3. Una vez generado el archivo `ubuntu_server_2410.box`, puedes **agregarlo a Vagrant**:
   ```bash
   vagrant box add ubuntu_server_2410.box --name ubuntu_server_2410
   ```
4. Comprueba que se haya registrado correctamente:
   ```bash
   vagrant box list
   ```
   - Debería aparecer la nueva box `ubuntu_server_2410 (virtualbox, 0)`, o similar.



## 6. Probar la box con un Vagrantfile mínimo

Para verificar que la nueva caja funciona y se configura bien la red, crea una carpeta de prueba con un archivo `Vagrantfile`. Por ejemplo:

```ruby
# Vagrantfile de prueba
Vagrant.configure("2") do |config|
  config.vm.box = "ubuntu_server_2410"
  
  # Prueba: asignar una IP privada fija
  config.vm.network "private_network", ip: "192.168.56.50"
  
  # Ajustar recursos, por ejemplo:
  config.vm.provider "virtualbox" do |vb|
    vb.memory = 2048
    vb.cpus = 2
  end

  # Desactivar inserción de nueva llave
  config.ssh.insert_key = false
end
```

Luego, en esa carpeta:

```bash
vagrant up
```


