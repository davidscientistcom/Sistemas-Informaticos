
```ruby
NUM_MACHINES = 4

Vagrant.configure("2") do |config|
  # Configuración común
  config.vm.box = "ubuntu/xenial64"
  
  # Configuración para evitar prompts de SSH
  config.ssh.insert_key = false
  config.ssh.private_key_path = ["#{Dir.home}/.ssh/vagrant_vm_key", "#{Dir.home}/.vagrant.d/insecure_private_key"]
  config.ssh.extra_args = ["-o StrictHostKeyChecking=no", "-o UserKnownHostsFile=/dev/null"]
  
  # Verificar si la clave SSH existe antes de continuar
  unless File.exist?("#{Dir.home}/.ssh/vagrant_vm_key.pub")
    puts "ERROR: La clave SSH no existe en #{Dir.home}/.ssh/vagrant_vm_key.pub"
    puts "Crea la clave con: ssh-keygen -t rsa -b 4096 -f ~/.ssh/vagrant_vm_key -N \"\""
    exit 1
  end
  
  # Crear múltiples máquinas
  (1..NUM_MACHINES).each do |i|
    config.vm.define "node-#{i}" do |node|
      # IP única para cada máquina
      node.vm.network "private_network", ip: "192.168.33.#{10+i}"
      
      # Configurar los recursos de hardware
      node.vm.provider "virtualbox" do |vb|
        vb.memory = "4096"  # 4 GB de memoria
        vb.cpus = 2         # 2 CPUs
        vb.name = "ansible-node-#{i}" # Nombre personalizado en VirtualBox
      end
      
      # Copia la clave pública generada en el host a la VM
      node.vm.provision "file", run: "once", source: "#{Dir.home}/.ssh/vagrant_vm_key.pub", destination: "/home/vagrant/.ssh/vagrant_vm_key.pub"
      
      # Configuración inicial del servidor SSH en dos pasos para mayor robustez
      # Paso 1: Preparación básica
      node.vm.provision "shell", run: "once", name: "ssh-setup-part1" do |s|
        s.inline = <<-SHELL
          echo "=== Configurando SSH (Parte 1) ==="
          # Actualizar repositorios
          apt-get update
          # Instalar OpenSSH Server si no está instalado
          if ! dpkg -l | grep -q openssh-server; then
            apt-get install -y openssh-server
          fi
          
          # Crear el directorio .ssh con permisos correctos
          mkdir -p /home/vagrant/.ssh
          chmod 700 /home/vagrant/.ssh
          chown vagrant:vagrant /home/vagrant/.ssh
          
          # Configurar el hostname
          echo "node-#{i}" > /etc/hostname
          hostname node-#{i}
          echo "127.0.0.1 node-#{i}" >> /etc/hosts
          
          # Cargar teclado español
          loadkeys es
        SHELL
      end
      
      # Paso 2: Configuración SSH avanzada
      node.vm.provision "shell", run: "once", name: "ssh-setup-part2" do |s|
        s.inline = <<-SHELL
          echo "=== Configurando SSH (Parte 2) ==="
          # Configurar el servidor SSH para aceptar claves públicas
          sed -i 's/#PubkeyAuthentication yes/PubkeyAuthentication yes/' /etc/ssh/sshd_config
          sed -i 's/#PasswordAuthentication yes/PasswordAuthentication no/' /etc/ssh/sshd_config
          
          # Verificar que la clave pública existe
          if [ ! -f /home/vagrant/.ssh/vagrant_vm_key.pub ]; then
            echo "ERROR: No se encontró la clave pública en la VM"
            exit 1
          fi
          
          # Sobrescribir el contenido de authorized_keys con la clave pública
          cat /home/vagrant/.ssh/vagrant_vm_key.pub > /home/vagrant/.ssh/authorized_keys
          
          # Ajustar permisos explícitamente
          chmod 600 /home/vagrant/.ssh/authorized_keys
          chown vagrant:vagrant /home/vagrant/.ssh/authorized_keys
          
          # Verificar la configuración SSH
          echo "=== Verificando configuración SSH ==="
          ls -la /home/vagrant/.ssh/
          cat /etc/ssh/sshd_config | grep PubkeyAuthentication
          cat /etc/ssh/sshd_config | grep PasswordAuthentication
          
          # Reiniciar el servidor SSH
          echo "=== Reiniciando SSH ==="
          systemctl restart sshd
          sleep 2
          # Verificar que SSH está funcionando
          systemctl status sshd
        SHELL
      end
    end
  end
  
  # Mensaje informativo al finalizar
  config.vm.provision "shell", run: "once", privileged: false, name: "final-message" do |s|
    s.inline = <<-SHELL
      echo ""
      echo "====================================================="
      echo "Configuración completada. Para conectarse:"
      echo "ssh -i ~/.ssh/vagrant_vm_key vagrant@192.168.33.XX"
      echo "donde XX es 11, 12, 13, etc. según el número de nodo"
      echo "====================================================="
    SHELL
  end
end
```