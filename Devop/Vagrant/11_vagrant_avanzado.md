
### Bucle para Crear N Máquinas en Vagrant

Puedes usar un bucle en el **Vagrantfile** para crear múltiples máquinas virtuales. Esto es posible gracias a que el **Vagrantfile** utiliza Ruby, lo que permite aprovechar las estructuras de control y bucles.

**Ejemplo de Bucle para Crear N Máquinas:**

```ruby
Vagrant.configure("2") do |config|
  (1..3).each do |i|
    config.vm.define "node#{i}" do |node|
      node.vm.box = "bento/ubuntu-23.10"
      node.vm.network "private_network", ip: "192.168.33.#{10 + i}"
      
      # Configuración de recursos
      node.vm.provider "virtualbox" do |vb|
        vb.memory = "1024"
        vb.cpus = 1
      end

      # Provisión para instalar SSH
      node.vm.provision "shell",run: "once", inline: <<-SHELL
        apt-get update
        apt-get install -y openssh-server
        systemctl restart sshd
      SHELL
    end
  end
end
```

En este ejemplo:

- Se crean 3 máquinas (`node1`, `node2`, `node3`).
- Cada máquina tiene una IP distinta (`192.168.33.11`, `192.168.33.12`, `192.168.33.13`).
- Las configuraciones son repetitivas y se simplifican gracias al bucle.



### Uso de Condicionales en Vagrant

Dado que el **Vagrantfile** es Ruby, puedes usar condicionales para ajustar la configuración en función de ciertas condiciones. Por ejemplo:

**Ejemplo: Configuración Condicional Basada en el Entorno**

```ruby
Vagrant.configure("2") do |config|
  # Variables de entorno
  is_production = ENV['ENV'] == 'production'

  config.vm.define "app" do |app|
    app.vm.box = "bento/ubuntu-23.10"
    app.vm.network "private_network", ip: is_production ? "192.168.50.10" : "192.168.33.10"

    app.vm.provider "virtualbox" do |vb|
      vb.memory = is_production ? "2048" : "1024" # Más RAM en producción
      vb.cpus = is_production ? 2 : 1
    end

    # Provisión condicional
    app.vm.provision "shell", inline: <<-SHELL
      echo "Entorno: #{is_production ? 'Producción' : 'Desarrollo'}"
    SHELL
  end
end
```

En este ejemplo:

- La configuración varía según la variable de entorno `ENV`.
- Si `ENV=production`, la máquina tiene más recursos y una IP diferente.
