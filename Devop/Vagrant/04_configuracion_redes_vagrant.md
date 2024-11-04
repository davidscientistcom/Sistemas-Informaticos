### 4. Configuración de Red en Vagrant

Configurar correctamente la red de una máquina virtual es esencial para garantizar la comunicación entre el host y la máquina, y para acceder a servicios de red desde fuera de la máquina. Vagrant ofrece varias opciones para configurar la red, lo que permite adaptar las máquinas a diferentes necesidades y escenarios.

#### 4.1 Opciones de Red en Vagrant

Vagrant ofrece tres tipos principales de redes:

1. **Red NAT (Network Address Translation)**: Esta es la configuración de red predeterminada en Vagrant. En una red NAT, Vagrant maneja automáticamente el acceso de la máquina virtual a internet. La máquina no es accesible directamente desde el host, a menos que se configuren reglas de reenvío de puertos.
   
2. **Red Privada (Private Network)**: Esta opción permite que la máquina virtual sea accesible desde el host mediante una IP fija en una red privada. Es útil cuando solo se necesita que el host pueda acceder a la máquina. En entornos educativos o de desarrollo, esta es una configuración común, ya que asegura la comunicación entre el host y la máquina virtual sin exponerse en la red local.

3. **Red Pública (Public Network)**: Esta configuración conecta la máquina virtual a la misma red que el host, lo que le permite ser accesible desde otros dispositivos en la red local. Es útil para escenarios en los que varios usuarios o dispositivos necesitan acceder a la máquina.

#### 4.2 Configuración de Red Privada

La red privada es una de las configuraciones más utilizadas en entornos de desarrollo y enseñanza, ya que permite que el host y la máquina virtual se comuniquen directamente mediante una IP fija. 

Para configurar una red privada en el **Vagrantfile**, puedes usar el siguiente código:

```ruby
Vagrant.configure("2") do |config|
  config.vm.box = "ubuntu/bionic64"
  config.vm.network "private_network", ip: "192.168.33.10"
end
```

En este ejemplo, se asigna la IP `192.168.33.10` a la máquina virtual en una red privada. Esto permite que puedas acceder a la máquina directamente desde el host usando esta IP.

> **Nota**: Si usas varias máquinas con red privada en Vagrant, asegúrate de asignar IPs distintas a cada máquina para evitar conflictos de red.

#### 4.3 Configuración de Red Pública

Para configurar la máquina en una red pública y asignarle una IP en la misma red que el host, puedes usar el siguiente código:

```ruby
Vagrant.configure("2") do |config|
  config.vm.box = "ubuntu/bionic64"
  config.vm.network "public_network"
end
```

Cuando utilices `public_network`, Vagrant intentará asignar una IP automáticamente en la red pública, aunque también puedes configurarla manualmente si lo necesitas. Esta configuración es útil si deseas que otros dispositivos en la misma red local puedan acceder a la máquina.

#### 4.4 Configuración de Reenvío de Puertos (Port Forwarding)

El reenvío de puertos permite mapear puertos del host a puertos de la máquina virtual. Esto es particularmente útil si estás utilizando la red NAT predeterminada, ya que no permite el acceso directo desde el host a menos que configures el reenvío de puertos.

**Ejemplo de configuración de reenvío de puertos en el Vagrantfile:**

```ruby
Vagrant.configure("2") do |config|
  config.vm.box = "ubuntu/bionic64"
  
  # Reenvío del puerto 8080 del host al puerto 80 de la máquina virtual
  config.vm.network "forwarded_port", guest: 80, host: 8080
end
```

En este caso, si tienes un servidor web ejecutándose en la máquina virtual en el puerto 80, podrás acceder a él desde el host mediante `localhost:8080`.

#### 4.5 Ejemplo Completo de Configuración de Red en Vagrantfile

A continuación, se presenta un **Vagrantfile** que incluye una configuración completa de red, combinando una red privada con una IP fija y un reenvío de puertos:

```ruby
Vagrant.configure("2") do |config|
  config.vm.box = "ubuntu/bionic64"
  
  # Red privada con IP fija
  config.vm.network "private_network", ip: "192.168.33.10"
  
  # Reenvío del puerto 8080 del host al puerto 80 de la máquina virtual
  config.vm.network "forwarded_port", guest: 80, host: 8080

  # Configuración de recursos de hardware
  config.vm.provider "virtualbox" do |vb|
    vb.memory = "2048"
    vb.cpus = 2
  end
end
```

En este ejemplo, la máquina virtual:

- Utiliza una red privada con IP `192.168.33.10`, permitiendo el acceso directo desde el host.
- Redirige el puerto 80 de la máquina virtual al puerto 8080 del host, facilitando el acceso a un servidor web en el host.

#### 4.6 Validación de la Configuración de Red

Una vez configurado el Vagrantfile, puedes verificar la configuración de red de la máquina siguiendo estos pasos:

1. Inicia la máquina virtual:
   ```bash
   vagrant up
   ```

2. Conéctate a la máquina mediante SSH:
   ```bash
   vagrant ssh
   ```

3. Comprueba la configuración de red de la máquina virtual ejecutando el comando `ifconfig` (en distribuciones de Linux) o `ip addr`. Esto mostrará la IP asignada a la máquina en la red privada.

4. Desde el host, intenta acceder a la máquina utilizando la IP fija o el puerto configurado para el reenvío de puertos.

> **Ejemplo**: Abre un navegador en el host e ingresa `http://192.168.33.10` para acceder a un servicio que se esté ejecutando en la máquina en la IP privada. Alternativamente, si configuraste el reenvío de puertos, accede a `http://localhost:8080`.

