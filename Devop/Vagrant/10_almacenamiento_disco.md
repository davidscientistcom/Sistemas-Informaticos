## ALMACENAMIENTO DE DISCO

En Vagrant, la asignación de **espacio en disco** no está incluida de forma nativa en la configuración estándar del **Vagrantfile**. Esto se debe a que el tamaño del disco de la máquina virtual está predeterminado por la **box** base que se utiliza. Sin embargo, en muchos casos, puede ser necesario ajustar este tamaño, especialmente para entornos que requieren grandes volúmenes de almacenamiento.

### 1. Ajuste de tamaño del disco.

El tamaño del disco en VirtualBox (y otros proveedores) está definido al crear la máquina virtual a partir de la box. Cambiar el tamaño del disco requiere modificar los parámetros de configuración subyacentes, algo que Vagrant no hace por defecto. Por esta razón, se necesita un plugin como **vagrant-disksize** para gestionar esta configuración.

**Sentido de usar un plugin**:
- **Consistencia**: Permite definir el tamaño del disco directamente en el **Vagrantfile**, asegurando que todas las máquinas creadas tengan el tamaño de disco correcto.
- **Facilidad de uso**: Evita pasos manuales adicionales (como modificar la máquina virtual en VirtualBox o usar herramientas externas).
- **Automatización**: El plugin ajusta el tamaño del disco automáticamente cuando se ejecuta `vagrant up`, lo que es ideal para entornos reproducibles.

### 2. ¿Cuánto Espacio en Disco Tiene por Defecto una Máquina en Vagrant?

El tamaño predeterminado del disco depende de la **box** utilizada. Por ejemplo:
- Las boxes de **Ubuntu oficiales** suelen tener un tamaño de disco predeterminado de **10 GB**.
- Algunas boxes pueden tener tamaños más grandes o pequeños, dependiendo de cómo fueron configuradas por sus creadores.

### 3. Tamaño Máximo del Disco en VirtualBox

El tamaño máximo del disco en VirtualBox depende del sistema de archivos del host y de la configuración de VirtualBox. En general:
- **VirtualBox** soporta discos de hasta **2 TB** en sistemas de archivos compatibles, como NTFS o ext4.
- El tamaño práctico dependerá del espacio libre en el disco del host.

### 4. Cómo Configurar el Tamaño del Disco con el Plugin `vagrant-disksize`

El plugin **`vagrant-disksize`** permite especificar el tamaño del disco de la máquina virtual directamente en el **Vagrantfile**. A continuación, se describen los pasos para instalar y usar el plugin.

#### Paso 1: Instalar el Plugin `vagrant-disksize`

Ejecuta el siguiente comando para instalar el plugin:

```bash
vagrant plugin install vagrant-disksize
```

#### Paso 2: Configurar el Tamaño del Disco en el Vagrantfile

Agrega la configuración del tamaño del disco al Vagrantfile. Por ejemplo:

```ruby
Vagrant.configure("2") do |config|
  # Configurar la box
  config.vm.box = "ubuntu/bionic64"
  
  # Configuración del tamaño del disco
  config.disksize.size = "50GB" # Tamaño del disco en GB
  
  # Configuración de recursos de hardware
  config.vm.provider "virtualbox" do |vb|
    vb.memory = "2048"
    vb.cpus = 2
  end
end
```

#### Paso 3: Crear o Recrear la Máquina Virtual

El tamaño del disco se configura **al crear la máquina virtual**, por lo que necesitas destruir y recrear la máquina si ya existe:

```bash
vagrant destroy
vagrant up
```

#### Nota Importante:
El cambio de tamaño solo afecta al disco virtual asignado a la máquina, pero no extiende automáticamente las particiones dentro del sistema operativo de la máquina virtual. Para usar el espacio adicional, debes **redimensionar las particiones** manualmente dentro del sistema operativo (por ejemplo, usando herramientas como `gparted` o comandos de `fdisk` y `resize2fs` en Linux).

### 5. Limitaciones y Consideraciones

- **Compatibilidad del Plugin**: El plugin **`vagrant-disksize`** es compatible principalmente con VirtualBox. Para otros proveedores, como VMware o AWS, debes usar métodos específicos de esos entornos.
- **Redimensionado de Particiones**: Aunque el plugin amplía el disco, el sistema operativo invitado debe reconocer y usar el nuevo tamaño. Esto requiere pasos adicionales en la mayoría de los casos.
- **Espacio en el Host**: Asegúrate de tener suficiente espacio en el disco del host antes de asignar un tamaño grande al disco de la máquina virtual.

### Ejemplo de Configuración Completa

```ruby
Vagrant.configure("2") do |config|
  # Definir la box base
  config.vm.box = "ubuntu/bionic64"
  
  # Configurar el tamaño del disco
  config.disksize.size = "100GB" # Tamaño deseado
  
  # Configurar la red privada
  config.vm.network "private_network", ip: "192.168.33.10"
  
  # Configurar los recursos de hardware
  config.vm.provider "virtualbox" do |vb|
    vb.memory = "4096"
    vb.cpus = 2
  end
end
```

Este ejemplo define una máquina virtual con:
- **100 GB de disco**
- **4 GB de RAM**
- **2 CPUs**
- Una IP estática en red privada.
