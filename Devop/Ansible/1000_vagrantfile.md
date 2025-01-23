Vagrant.configure("2") do |config|
  # Define una caja base para todas las máquinas
  config.vm.box = "ubuntu/bionic64"

  (1..4).each do |i|
    config.vm.define "vm#{i}" do |vm_config|
      # Configuración básica de la VM
      vm_config.vm.hostname = "vm#{i}"
      vm_config.vm.network "private_network", ip: "192.168.33.#{10 + i}"

      # Configuración de hardware
      vm_config.vm.provider "virtualbox" do |vb|
        vb.memory = "4096"  # 4 GB de memoria
        vb.cpus = 2         # 2 CPUs
      end

      # Copia la clave pública generada en el host a la VM
      vm_config.vm.provision "file", source: "#{Dir.home}/.ssh/vagrant_vm_key.pub", destination: "/home/vagrant/.ssh/vagrant_vm_key.pub"

      # Configuración inicial del servidor SSH
      vm_config.vm.provision "shell", run: "once" do |s|
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
      vm_config.ssh.insert_key = false
      vm_config.ssh.extra_args = ["-o StrictHostKeyChecking=no", "-o UserKnownHostsFile=/dev/null"]
    end
  end
end
