### Configuración de Vagrant con SSH para Ansible en WSL

Este anexo detalla, paso a paso, cómo configurar una máquina virtual en **Vagrant** bajo **Windows** (usando VirtualBox como proveedor), con la box de Ubuntu Server 23.10. La máquina estará lista para ser gestionada mediante **Ansible** desde **WSL** (Windows Subsystem for Linux). Incluye la generación de claves SSH en WSL y la configuración necesaria para que Ansible pueda comunicarse con la máquina.



#### **1. Requisitos Previos**

Antes de comenzar, asegúrate de tener lo siguiente instalado y configurado en tu sistema:

1. **Windows**:
   - Vagrant ([descarga aquí](https://www.vagrantup.com/downloads)).
   - VirtualBox ([descarga aquí](https://www.virtualbox.org)).
   - WSL2 configurado y ejecutando una distribución de Linux compatible (como Ubuntu).

2. **WSL**:
   - `ansible` instalado en tu entorno WSL. Si no está instalado:
     ```bash
     sudo apt update && sudo apt install ansible -y
     ```

3. **SSH habilitado en WSL**:
   - Asegúrate de tener el comando `ssh` disponible en tu distribución WSL.



#### **2. Configuración de la Máquina Virtual en Vagrant**

Sigue estos pasos para configurar una máquina virtual con Vagrant.

##### **Paso 1: Crear un Directorio para el Proyecto**

1. Abre una terminal en Windows (PowerShell o CMD).
2. Crea un directorio para el proyecto:
   ```bash
   mkdir vagrant_ansible_project
   cd vagrant_ansible_project
   ```

##### **Paso 2: Inicializar Vagrant**

Inicializa el proyecto con la box **Bento Ubuntu Server 23.10**:

```bash
vagrant init bento/ubuntu-23.10
```

Este comando generará un archivo `Vagrantfile` en el directorio actual.

##### **Paso 3: Configurar el Vagrantfile**

Edita el archivo `Vagrantfile` para incluir las siguientes configuraciones mínimas:

```ruby
Vagrant.configure("2") do |config|
  # Box de Ubuntu Server 23.10
  config.vm.box = "bento/ubuntu-23.10"
  
  # Configuración de red privada con IP fija
  config.vm.network "private_network", ip: "192.168.33.10"
  
  # Configuración de SSH para acceso desde WSL
  config.vm.provision "shell", inline: <<-SHELL
    apt-get update
    apt-get install -y openssh-server
    sed -i 's/#PasswordAuthentication yes/PasswordAuthentication yes/' /etc/ssh/sshd_config
    systemctl restart sshd
  SHELL

  # Configuración de recursos de hardware
  config.vm.provider "virtualbox" do |vb|
    vb.memory = "1024"  # 1 GB de RAM
    vb.cpus = 1         # 1 CPU
  end
end
```

Este Vagrantfile realiza lo siguiente:

- Asigna la box `bento/ubuntu-23.10`.
- Configura una red privada con la IP `192.168.33.10`.
- Instala y habilita el servidor SSH, permitiendo autenticación por contraseña.
- Asigna 1 GB de RAM y 1 CPU a la máquina virtual.

##### **Paso 4: Crear y Arrancar la Máquina Virtual**

Ejecuta el siguiente comando para crear y arrancar la máquina virtual:

```bash
vagrant up
```

> **Nota**: La primera vez que ejecutes este comando, Vagrant descargará la box, lo que puede tomar tiempo.



#### **3. Configuración de la Clave SSH en WSL**

##### **Paso 1: Generar una Clave SSH en WSL**

Abre tu terminal WSL y genera una clave SSH para autenticarte en la máquina virtual:

```bash
ssh-keygen -t rsa -b 4096
```

- Cuando se te pregunte por el archivo en el que guardar la clave, usa el valor predeterminado (`~/.ssh/id_rsa`).
- No configures una contraseña (deja el campo vacío).

##### **Paso 2: Copiar la Clave Pública a la Máquina Virtual**

Utiliza el comando `ssh-copy-id` para copiar la clave pública al nodo gestionado (máquina virtual):

```bash
ssh-copy-id -i ~/.ssh/id_rsa vagrant@192.168.33.10
```

- **Usuario**: `vagrant` (predeterminado en Vagrant).
- **Contraseña**: `vagrant` (también predeterminada).

##### **Paso 3: Verificar el Acceso SSH**

Prueba la conexión SSH desde WSL para asegurarte de que puedes acceder a la máquina virtual sin contraseña:

```bash
ssh vagrant@192.168.33.10
```

Si todo está configurado correctamente, deberías iniciar sesión sin necesidad de ingresar una contraseña.



#### **4. Configuración del Inventario de Ansible**

##### **Paso 1: Crear un Archivo de Inventario**

En tu terminal WSL, crea un archivo de inventario llamado `hosts` en un directorio de trabajo para Ansible:

```bash
mkdir ~/ansible_project
cd ~/ansible_project
nano hosts
```

Agrega lo siguiente al archivo:

```plaintext
[servidores]
192.168.33.10 ansible_user=vagrant ansible_ssh_private_key_file=~/.ssh/id_rsa
```

- `192.168.33.10`: Dirección IP de la máquina virtual.
- `ansible_user`: Usuario SSH (`vagrant`).
- `ansible_ssh_private_key_file`: Ruta a la clave privada generada en WSL.

Guarda y cierra el archivo.

##### **Paso 2: Probar la Conexión con Ansible**

Usa el módulo `ping` para probar la conexión:

```bash
ansible servidores -i hosts -m ping
```

Si la configuración es correcta, deberías ver una salida similar a esta:

```plaintext
192.168.33.10 | SUCCESS => {
    "changed": false,
    "ping": "pong"
}
```

