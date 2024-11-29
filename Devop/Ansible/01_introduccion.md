### 1. Introducción a Ansible

Ansible es una herramienta de automatización de código abierto que permite gestionar y configurar sistemas de manera sencilla, escalable y eficiente. Su diseño sin agentes y su uso de SSH para comunicarse con los nodos lo convierten en una opción ideal para entornos de administración de sistemas, desarrollo y enseñanza.

#### 1.1 ¿Qué es Ansible?

Ansible es una herramienta diseñada para automatizar tareas repetitivas y complejas relacionadas con la administración de sistemas y la implementación de aplicaciones. Esto incluye:

- Configuración de sistemas operativos y servicios.
- Despliegue de aplicaciones.
- Gestión de infraestructura en múltiples entornos.

A diferencia de otras herramientas de automatización, Ansible no requiere la instalación de agentes en los sistemas gestionados, lo que simplifica su configuración y mantenimiento.

**Arquitectura de Ansible:**
- **Nodo de control**: Es el sistema desde el cual se ejecutan los comandos y playbooks de Ansible. Este sistema necesita tener instalado Ansible.
- **Nodos gestionados**: Son los sistemas que se configuran o gestionan mediante Ansible. Solo necesitan permitir acceso SSH y contar con Python instalado.

#### 1.2 Ventajas de Ansible

1. **Sin agentes**: Ansible no requiere software adicional en los nodos gestionados, ya que utiliza SSH para comunicarse.
2. **Simplicidad**: Los scripts de Ansible (conocidos como playbooks) están escritos en YAML, un formato de datos sencillo y legible.
3. **Escalabilidad**: Permite gestionar cientos o miles de servidores desde un único nodo de control.
4. **Reproducibilidad**: Las configuraciones y tareas se definen como código, lo que asegura que los cambios sean consistentes y reproducibles.
5. **Extensibilidad**: Ansible cuenta con una gran cantidad de módulos que permiten realizar tareas como instalar paquetes, configurar servicios, gestionar bases de datos, y mucho más.

#### 1.3 Instalación de Ansible

Para empezar a trabajar con Ansible, primero debemos instalarlo en el nodo de control. Este nodo será el sistema desde el cual se gestionarán los nodos remotos.

##### Instalación en Ubuntu

1. **Actualizar el sistema**:

   ```bash
   sudo apt update && sudo apt upgrade -y
   ```

2. **Añadir el repositorio de Ansible**:

   ```bash
   sudo apt install software-properties-common -y
   sudo add-apt-repository --yes --update ppa:ansible/ansible
   ```

3. **Instalar Ansible**:

   ```bash
   sudo apt install ansible -y
   ```

4. **Verificar la instalación**:

   ```bash
   ansible --version
   ```

   Esto debería mostrar la versión instalada de Ansible.

##### Instalación en Otras Distribuciones

- **CentOS/Red Hat**:

  ```bash
  sudo yum install epel-release -y
  sudo yum install ansible -y
  ```

- **macOS** (usando Homebrew):

  ```bash
  brew install ansible
  ```

- **Windows**: Ansible no se instala directamente en Windows, pero puedes usar el Subsistema de Windows para Linux (WSL) o una máquina virtual como nodo de control.

#### 1.4 Comprobación de Conexión SSH

Para que Ansible pueda gestionar los nodos, es necesario configurar el acceso SSH entre el nodo de control y los nodos gestionados. Sigue estos pasos:

1. **Generar una clave SSH en el nodo de control** (si no existe):

   ```bash
   ssh-keygen -t rsa -b 4096
   ```

   Acepta las opciones predeterminadas y recuerda la ubicación de la clave generada (normalmente en `~/.ssh/id_rsa`).

2. **Copiar la clave pública al nodo gestionado**:

   ```bash
   ssh-copy-id usuario@nodo_gestionado
   ```

   Esto asegura que el nodo de control pueda acceder al nodo gestionado sin requerir contraseña.

3. **Probar la conexión SSH**:

   ```bash
   ssh usuario@nodo_gestionado
   ```

   Si la conexión es exitosa, Ansible podrá gestionar el nodo.

#### 1.5 Verificación de los Nodos Gestionados

Una vez configurado el acceso SSH, podemos probar la comunicación entre el nodo de control y los nodos gestionados utilizando el comando `ansible` con el módulo `ping`:

1. Crear un archivo de inventario llamado `hosts` con el siguiente contenido:

   ```plaintext
   [servidores]
   nodo1 ansible_host=192.168.33.10
   nodo2 ansible_host=192.168.33.11
   ```

   En este archivo:
   - `servidores` es un grupo que agrupa a los nodos gestionados.
   - `ansible_host` especifica la dirección IP o nombre de host de cada nodo.

2. Ejecutar el comando de prueba:

   ```bash
   ansible servidores -i hosts -m ping
   ```

   Esto enviará un ping a cada nodo del grupo `servidores`. Si la configuración es correcta, deberías ver una salida similar a esta:

   ```plaintext
   nodo1 | SUCCESS => {
       "changed": false,
       "ping": "pong"
   }
   nodo2 | SUCCESS => {
       "changed": false,
       "ping": "pong"
   }
   ```