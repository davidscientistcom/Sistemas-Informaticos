### 5. Personalización de la Máquina Virtual

La personalización de una máquina virtual en Vagrant permite adaptar el entorno según los requisitos específicos del proyecto. Vagrant facilita el ajuste de diversos parámetros, como los recursos de hardware (memoria y CPU), la sincronización de carpetas y la configuración de puertos para el acceso a servicios. Estas personalizaciones son especialmente útiles en entornos de desarrollo y educativos, donde la flexibilidad y el control del entorno son esenciales.

#### 5.1 Asignación de Recursos de Hardware

Vagrant permite configurar la cantidad de **memoria** y **CPU** asignadas a la máquina virtual. Esto puede hacerse mediante la configuración del proveedor (por ejemplo, VirtualBox) en el Vagrantfile. La siguiente configuración es un ejemplo de cómo asignar 2 GB de memoria RAM y 2 CPUs a la máquina:

```ruby
Vagrant.configure("2") do |config|
  config.vm.box = "ubuntu/bionic64"
  
  # Configuración de recursos para VirtualBox
  config.vm.provider "virtualbox" do |vb|
    vb.memory = "2048"  # Asignar 2 GB de memoria
    vb.cpus = 2         # Asignar 2 CPUs
  end
end
```

En este ejemplo:

- **`vb.memory`** define la cantidad de memoria RAM (en MB) asignada a la máquina.
- **`vb.cpus`** define el número de CPUs asignadas a la máquina virtual.

Este tipo de configuración es útil para asegurar que la máquina virtual disponga de suficientes recursos para ejecutar aplicaciones o servicios que puedan consumir recursos intensivamente, como bases de datos o servidores web.

#### 5.2 Sincronización de Carpetas (Folder Sync)

La sincronización de carpetas permite compartir una carpeta en el host con la máquina virtual, facilitando el acceso a los archivos de desarrollo. Por ejemplo, si quieres que el código desarrollado en el host esté accesible dentro de la máquina virtual, puedes utilizar la configuración de sincronización de carpetas en el Vagrantfile:

```ruby
Vagrant.configure("2") do |config|
  config.vm.box = "ubuntu/bionic64"
  
  # Sincronización de carpetas entre host y máquina virtual
  config.vm.synced_folder "./mi_carpeta", "/vagrant_data"
end
```

En este ejemplo:

- `./mi_carpeta` es la ruta de la carpeta en el host que quieres sincronizar. Esta puede ser una carpeta con código fuente o archivos que necesitas compartir con la máquina virtual.
- `/vagrant_data` es la ruta en la máquina virtual donde se montará la carpeta.

> **Nota**: La sincronización de carpetas es muy útil para desarrollo, ya que permite editar archivos en el host con tu editor preferido y ver los cambios reflejados inmediatamente en la máquina virtual.

#### 5.3 Configuración de Puertos para Acceso a Servicios

Además del reenvío de puertos que ya mencionamos en la sección de redes, Vagrant permite configurar puertos adicionales para exponer servicios en la máquina virtual. Esto es útil si quieres acceder a varios servicios (como servidores web o bases de datos) desde el host.

**Ejemplo de configuración de varios puertos en el Vagrantfile:**

```ruby
Vagrant.configure("2") do |config|
  config.vm.box = "ubuntu/bionic64"
  
  # Reenvío del puerto 80 para un servidor web
  config.vm.network "forwarded_port", guest: 80, host: 8080
  
  # Reenvío del puerto 3306 para una base de datos MySQL
  config.vm.network "forwarded_port", guest: 3306, host: 3307
end
```

En este ejemplo:

- El puerto 80 de la máquina virtual se mapea al puerto 8080 del host, permitiendo acceder a un servidor web en la máquina virtual desde `localhost:8080`.
- El puerto 3306, donde comúnmente se ejecuta un servidor MySQL, se mapea al puerto 3307 en el host. Esto permite acceder a la base de datos MySQL desde el host sin interferir con otros servicios locales.

#### 5.4 Ejemplos de Vagrantfile con Configuraciones Avanzadas de Recursos

Para facilitar el trabajo en entornos con múltiples servicios y configuraciones personalizadas, veamos un ejemplo de un Vagrantfile avanzado que combina varios elementos: una red privada, sincronización de carpetas y la asignación de recursos de hardware.

```ruby
Vagrant.configure("2") do |config|
  config.vm.box = "ubuntu/bionic64"
  
  # Configuración de red privada con IP fija
  config.vm.network "private_network", ip: "192.168.33.10"
  
  # Sincronización de carpetas entre host y máquina virtual
  config.vm.synced_folder "./codigo", "/home/vagrant/codigo"
  
  # Configuración de recursos de hardware
  config.vm.provider "virtualbox" do |vb|
    vb.memory = "4096"  # 4 GB de memoria
    vb.cpus = 4         # 4 CPUs
  end

  # Reenvío de puertos para acceder a servicios en la máquina virtual
  config.vm.network "forwarded_port", guest: 80, host: 8080
  config.vm.network "forwarded_port", guest: 3306, host: 3307
end
```

En este Vagrantfile:

- Se asigna una IP fija en red privada.
- Se sincroniza una carpeta llamada `./codigo` en el host con la ruta `/home/vagrant/codigo` en la máquina virtual.
- Se asignan 4 GB de memoria RAM y 4 CPUs.
- Se configuran dos puertos reenviados: uno para un servidor web (80 a 8080) y otro para una base de datos MySQL (3306 a 3307).

#### 5.5 Validación de la Configuración de Personalización

Para verificar que los cambios en el Vagrantfile se apliquen correctamente, puedes seguir estos pasos:

1. **Reinicia la máquina virtual** para aplicar cualquier cambio en el Vagrantfile:
   ```bash
   vagrant reload
   ```

2. **Conéctate mediante SSH** y verifica la configuración de hardware y sincronización de carpetas:
   ```bash
   vagrant ssh
   ```

3. **Confirma la asignación de memoria y CPU**: En la máquina virtual, puedes usar el comando `top` o `htop` para ver los recursos asignados.

4. **Comprueba la sincronización de carpetas**: Ve a la carpeta configurada dentro de la máquina virtual (`/home/vagrant/codigo` en el ejemplo) y verifica que los archivos del host se encuentran allí.

5. **Prueba el acceso a los servicios configurados**: Abre un navegador en el host y verifica que puedes acceder a los servicios en los puertos especificados (por ejemplo, `http://localhost:8080` para el servidor web).

