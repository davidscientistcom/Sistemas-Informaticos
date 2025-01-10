### 2. **Instalación de Ansible**

En esta sección, los alumnos configurarán Ansible en sus máquinas virtuales proporcionadas. Se detalla el proceso para sistemas basados en Debian, específicamente Ubuntu, pero se pueden mencionar otras distribuciones si es necesario.



#### 2.1. Requisitos previos

Antes de instalar Ansible, asegúrate de cumplir con los siguientes requisitos:

1. **Sistema operativo compatible**:
   - Ubuntu/Debian o cualquier distribución Linux que soporte Python.
2. **Python instalado**:
   - Ansible requiere Python (versión 3.8 o superior) en el nodo controlador y en los nodos gestionados.
3. **Acceso por SSH**:
   - Las máquinas gestionadas deben tener SSH habilitado y estar configuradas para aceptar claves públicas (esto ya está configurado en las máquinas virtuales proporcionadas).



#### 2.2. Instalación en sistemas Ubuntu/Debian

1. **Actualizar los paquetes del sistema**:

   ```bash
   sudo apt update && sudo apt upgrade -y
   ```

2. **Instalar los paquetes necesarios**:

   ```bash
   sudo apt install -y software-properties-common
   ```

3. **Agregar el repositorio oficial de Ansible**:

   ```bash
   sudo add-apt-repository --yes --update ppa:ansible/ansible
   ```

4. **Instalar Ansible**:

   ```bash
   sudo apt install -y ansible
   ```

5. **Verificar la instalación**:

   Comprueba que Ansible se instaló correctamente ejecutando:

   ```bash
   ansible --version
   ```

   Deberías obtener información sobre la versión instalada.



#### 2.3. Configuración inicial

Después de instalar Ansible, realiza los siguientes pasos para preparar el entorno:

1. **Configurar un archivo de inventario básico**:

   Crea el archivo de inventario en `/etc/ansible/hosts` (puedes usar otro directorio si lo prefieres):

   ```bash
   sudo nano /etc/ansible/hosts
   ```

   Agrega las direcciones IP de las máquinas gestionadas, como en este ejemplo:

   ```ini
   [webservers]
   192.168.56.10 ansible_user=vagrant ansible_ssh_private_key_file=~/.ssh/vagrant_vm_key
   ```

2. **Probar la conectividad**:

   Usa el comando `ping` de Ansible para verificar que puedes conectarte a los nodos gestionados:

   ```bash
   ansible all -m ping
   ```

   Si todo está configurado correctamente, deberías ver una salida indicando `SUCCESS`.



#### 2.4. Verificación de la instalación

**Ejercicio práctico: Prueba básica de Ansible**

1. Ejecuta el siguiente comando para confirmar que Ansible está funcionando:
   ```bash
   ansible all -a "uname -a"
   ```

   Esto debería devolver información del sistema operativo de los nodos gestionados.

2. Corrige posibles problemas si no funciona:
   - Verifica que la clave SSH esté correctamente configurada.
   - Asegúrate de que la IP de los nodos gestionados esté en el archivo de inventario.

