### 8. Ejemplos Prácticos y Casos de Uso

En esta sección, exploraremos configuraciones avanzadas y casos de uso prácticos que muestran el poder y la flexibilidad de Vagrant en la creación y administración de entornos de desarrollo. Estos ejemplos incluyen la creación de múltiples máquinas virtuales en un solo Vagrantfile y configuraciones de red personalizadas que son útiles para simular entornos de producción o configuraciones de red complejas.

#### 8.1 Creación de Múltiples Máquinas Virtuales en un Solo Vagrantfile

Vagrant permite definir varias máquinas virtuales en un solo Vagrantfile. Esto es útil en escenarios donde necesitas simular una red de varios servidores, como en aplicaciones distribuidas, pruebas de infraestructura o entornos de desarrollo que requieren servidores de base de datos, servidores web y balanceadores de carga.

##### Ejemplo de Vagrantfile para Múltiples Máquinas

En este ejemplo, crearemos dos máquinas virtuales: una que actúa como **servidor web** y otra como **servidor de base de datos**. Ambas estarán configuradas en una red privada para permitir la comunicación directa entre ellas.

```ruby
Vagrant.configure("2") do |config|
  # Configuración de la primera máquina: Servidor Web
  config.vm.define "web" do |web|
    web.vm.box = "ubuntu/bionic64"
    web.vm.hostname = "web-server"
    web.vm.network "private_network", ip: "192.168.33.11"
    
    # Configuración de recursos para el servidor web
    web.vm.provider "virtualbox" do |vb|
      vb.memory = "1024"
      vb.cpus = 1
    end
    
    # Reenvío de puerto para acceder al servidor web desde el host
    web.vm.network "forwarded_port", guest: 80, host: 8080
  end

  # Configuración de la segunda máquina: Servidor de Base de Datos
  config.vm.define "db" do |db|
    db.vm.box = "ubuntu/bionic64"
    db.vm.hostname = "db-server"
    db.vm.network "private_network", ip: "192.168.33.12"
    
    # Configuración de recursos para el servidor de base de datos
    db.vm.provider "virtualbox" do |vb|
      vb.memory = "1024"
      vb.cpus = 1
    end
    
    # Provisión para instalar MySQL en el servidor de base de datos
    db.vm.provision "shell", inline: <<-SHELL
      apt-get update
      apt-get install -y mysql-server
    SHELL
  end
end
```

En este Vagrantfile:

- **Máquina `web`**: Actúa como servidor web, configurada con una IP privada `192.168.33.11` y un puerto reenviado para acceso desde el host. El puerto 80 de la máquina se mapea al puerto 8080 del host.
- **Máquina `db`**: Actúa como servidor de base de datos, configurada con una IP privada `192.168.33.12`. También incluye un script de provisión que instala MySQL para simular una configuración de servidor de base de datos.

> **Nota**: Estas dos máquinas virtuales están en la misma red privada, lo que permite la comunicación entre ellas mediante sus IPs locales (`192.168.33.11` y `192.168.33.12`). Esto permite, por ejemplo, que el servidor web pueda conectarse al servidor de base de datos sin necesidad de configuraciones adicionales.

#### 8.2 Ejemplo de Vagrantfile para un Entorno Multi-Máquina con Balanceador de Carga

En entornos de producción, es común tener múltiples servidores web detrás de un balanceador de carga. Este ejemplo muestra cómo configurar un entorno de desarrollo con dos servidores web y un balanceador de carga.

```ruby
Vagrant.configure("2") do |config|
  # Configuración del balanceador de carga
  config.vm.define "loadbalancer" do |lb|
    lb.vm.box = "ubuntu/bionic64"
    lb.vm.hostname = "load-balancer"
    lb.vm.network "private_network", ip: "192.168.33.10"
    
    lb.vm.provider "virtualbox" do |vb|
      vb.memory = "512"
      vb.cpus = 1
    end
    
    # Provisión para instalar y configurar Nginx como balanceador de carga
    lb.vm.provision "shell", inline: <<-SHELL
      apt-get update
      apt-get install -y nginx
      echo "upstream web_servers {
        server 192.168.33.11;
        server 192.168.33.12;
      }
      server {
        listen 80;
        location / {
          proxy_pass http://web_servers;
        }
      }" > /etc/nginx/sites-available/default
      systemctl restart nginx
    SHELL
  end

  # Configuración del primer servidor web
  config.vm.define "web1" do |web1|
    web1.vm.box = "ubuntu/bionic64"
    web1.vm.hostname = "web-server-1"
    web1.vm.network "private_network", ip: "192.168.33.11"
    
    web1.vm.provider "virtualbox" do |vb|
      vb.memory = "512"
      vb.cpus = 1
    end
  end

  # Configuración del segundo servidor web
  config.vm.define "web2" do |web2|
    web2.vm.box = "ubuntu/bionic64"
    web2.vm.hostname = "web-server-2"
    web2.vm.network "private_network", ip: "192.168.33.12"
    
    web2.vm.provider "virtualbox" do |vb|
      vb.memory = "512"
      vb.cpus = 1
    end
  end
end
```

En este Vagrantfile:

- **Balanceador de carga (`loadbalancer`)**: Configurado con Nginx para distribuir el tráfico entre los dos servidores web. Nginx se configura para hacer balanceo de carga hacia los servidores web con IPs `192.168.33.11` y `192.168.33.12`.
- **Servidores web (`web1` y `web2`)**: Ambos servidores están en la misma red privada y tienen configuraciones idénticas, con IPs fijas.

> **Nota**: Este tipo de configuración es ideal para probar el balanceo de carga en entornos de desarrollo. Puedes acceder al balanceador de carga en `192.168.33.10` desde el host y ver cómo distribuye el tráfico entre los dos servidores web.

#### 8.3 Ejemplo de Configuración de Red para Pruebas de Ansible

Para entornos educativos, donde se practica el uso de Ansible para la provisión de infraestructura, es útil configurar una máquina que actúe como nodo de control y otra como nodo gestionado. A continuación, un ejemplo de Vagrantfile para este propósito.

```ruby
Vagrant.configure("2") do |config|
  # Nodo de control Ansible
  config.vm.define "control" do |control|
    control.vm.box = "ubuntu/bionic64"
    control.vm.network "private_network", ip: "192.168.33.20"
    
    control.vm.provider "virtualbox" do |vb|
      vb.memory = "1024"
      vb.cpus = 1
    end

    # Provisión para instalar Ansible en el nodo de control
    control.vm.provision "shell", inline: <<-SHELL
      apt-get update
      apt-get install -y ansible
    SHELL
  end

  # Nodo gestionado
  config.vm.define "node" do |node|
    node.vm.box = "ubuntu/bionic64"
    node.vm.network "private_network", ip: "192.168.33.21"
    
    node.vm.provider "virtualbox" do |vb|
      vb.memory = "512"
      vb.cpus = 1
    end

    # Configuración de SSH para que el nodo de control pueda gestionar el nodo
    node.vm.provision "shell", inline: <<-SHELL
      apt-get update
      apt-get install -y openssh-server
    SHELL
  end
end
```

En este Vagrantfile:

- **Nodo de Control (`control`)**: Se configura para instalar Ansible, y actúa como el nodo desde el que se gestionarán otras máquinas.
- **Nodo Gestionado (`node`)**: Configurado con SSH, permite ser administrado por Ansible. Ambos nodos están en una red privada y pueden comunicarse entre sí a través de sus IPs `192.168.33.20` y `192.168.33.21`.

Este entorno es ideal para practicar el uso de Ansible y aprender a ejecutar playbooks en entornos de prueba.
