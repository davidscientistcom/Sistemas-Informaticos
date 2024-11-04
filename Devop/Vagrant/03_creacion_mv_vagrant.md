### 3. Creación de Máquinas Virtuales con Vagrant

En esta sección, aprenderemos a crear máquinas virtuales en Vagrant utilizando configuraciones básicas y avanzadas en el archivo **Vagrantfile**. Nos enfocaremos en cómo definir y personalizar una máquina virtual para adaptarla a las necesidades de un entorno de desarrollo o educativo.

#### 3.1 Definición y Configuración de una Máquina Virtual

Crear una máquina virtual en Vagrant es un proceso sencillo, ya que el archivo **Vagrantfile** permite especificar todos los detalles necesarios para definir y configurar la máquina. Los elementos básicos que debes configurar en el Vagrantfile incluyen:

1. **Box (Caja)**: Define el sistema operativo base. Puedes encontrar múltiples boxes en el repositorio de Vagrant, tanto oficiales como de la comunidad. Por ejemplo, para crear una máquina con Ubuntu 18.04, el código sería:
   
   ```ruby
   Vagrant.configure("2") do |config|
     config.vm.box = "ubuntu/bionic64"
   end
   ```

2. **Asignación de Memoria y CPU**: Puedes personalizar los recursos de hardware asignados a la máquina virtual. Esto es especialmente útil cuando tienes que ajustar el rendimiento según el hardware de tu equipo. El siguiente ejemplo asigna 2 GB de RAM y 2 CPUs a la máquina:

   ```ruby
   Vagrant.configure("2") do |config|
     config.vm.box = "ubuntu/bionic64"
     config.vm.provider "virtualbox" do |vb|
       vb.memory = "2048"
       vb.cpus = 2
     end
   end
   ```

3. **Configuración de Red Básica**: Vagrant ofrece diversas opciones de red para permitir la comunicación entre el host y la máquina virtual. Las configuraciones más comunes incluyen:

   - **Red NAT (Network Address Translation)**: Vagrant asigna automáticamente una IP en una red privada y maneja la conexión a internet de la máquina. No requiere configuraciones adicionales en el Vagrantfile.
   - **Red Privada**: Esta opción permite establecer una IP estática para que la máquina virtual sea accesible solo desde el host.

   Por ejemplo, para asignar una IP estática (192.168.33.10) en una red privada, se utilizaría la siguiente configuración:

   ```ruby
   Vagrant.configure("2") do |config|
     config.vm.box = "ubuntu/bionic64"
     config.vm.network "private_network", ip: "192.168.33.10"
   end
   ```

#### 3.2 Ejemplo de Configuración Inicial en el Vagrantfile

Para comprender mejor cómo funcionan estas configuraciones, veamos un ejemplo completo de Vagrantfile que cubre una configuración básica de una máquina virtual con Ubuntu, recursos personalizados y una red privada.

**Ejemplo de Vagrantfile Mínimo para Crear una Máquina Virtual Básica**

```ruby
Vagrant.configure("2") do |config|
  # Box de Ubuntu 18.04
  config.vm.box = "ubuntu/bionic64"
  
  # Configuración de red privada con IP estática
  config.vm.network "private_network", ip: "192.168.33.10"

  # Configuración de recursos de hardware
  config.vm.provider "virtualbox" do |vb|
    vb.memory = "2048"    # 2 GB de RAM
    vb.cpus = 2           # 2 CPUs
  end
end
```

Este Vagrantfile define una máquina virtual con:

- **Box**: Ubuntu 18.04 (bionic64)
- **Red**: Una IP estática en red privada para acceso directo desde el host (IP: 192.168.33.10)
- **Recursos**: 2 GB de memoria y 2 CPUs asignados a la máquina virtual

#### 3.3 Creación de la Máquina Virtual

Una vez que el Vagrantfile está configurado, crear la máquina virtual es tan simple como ejecutar el comando `vagrant up` en el directorio donde se encuentra el Vagrantfile:

```bash
vagrant up
```

Este comando:

1. Descarga la box especificada si aún no está en el sistema.
2. Configura la máquina virtual con las propiedades definidas en el Vagrantfile.
3. Inicia la máquina virtual.

> **Nota**: Durante la primera ejecución, el proceso puede tardar un poco más, ya que se descarga la imagen de la box desde el repositorio de Vagrant.

Una vez que el proceso termina, puedes conectarte a la máquina utilizando SSH:

```bash
vagrant ssh
```

Este comando establece una conexión SSH directa con la máquina virtual, permitiéndote interactuar con el sistema operativo de la máquina creada.

#### 3.4 Personalización de la Máquina Virtual con Ejemplos Avanzados

Si necesitas configuraciones adicionales, Vagrant te permite personalizar la máquina virtual de varias maneras, como aumentar los recursos, añadir configuraciones de red adicionales o cambiar el proveedor de virtualización. En el siguiente apartado, abordaremos más detalles sobre configuraciones avanzadas, incluyendo el manejo de múltiples máquinas en un solo Vagrantfile.
