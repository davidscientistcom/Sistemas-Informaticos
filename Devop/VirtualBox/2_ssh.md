## Configuración Completa de SSH con Clave Pública desde Cero

Este proceso detalla cómo configurar el acceso SSH en una máquina **Ubuntu Server** en **VirtualBox** con autenticación por clave desde un sistema Windows, sin acceso inicial por SSH.

### Objetivo
Poder conectarnos desde windows  a una máquina virtual Ubuntu Server configurada en VirtualBox, utilizando autenticación por clave SSH.

### Prerrequisitos
- **Máquina virtual Ubuntu Server** instalada en **VirtualBox**.
- **IP estática configurada** en la máquina virtual para que sea accesible desde el host.

---

### Paso 1: Instalación y Configuración Inicial del Servidor SSH en Ubuntu Server

1. **Accede a la máquina virtual en VirtualBox** y abre la consola.
2. **Actualiza los paquetes del sistema** (opcional pero recomendado):
   ```bash
   sudo apt update && sudo apt upgrade -y
   ```
3. **Instala OpenSSH Server**:
   ```bash
   sudo apt install openssh-server -y
   ```
4. **Verifica que el servidor SSH esté activo**:
   ```bash
   sudo systemctl status ssh
   ```
   Si no está activo, puedes iniciarlo manualmente:
   ```bash
   sudo systemctl start ssh
   ```

---

### Paso 2: Generar el Par de Claves en Windows (en la Máquina del Estudiante)

Cada estudiante generará su propio par de claves SSH en su sistema Windows para autenticarse en la máquina Ubuntu Server.

1. **Abrir PowerShell o Git Bash** en el sistema Windows del estudiante.
2. **Generar el par de claves SSH** usando una ruta específica para asegurar la correcta generación en Windows:
   ```powershell
   ssh-keygen -t rsa -b 4096 -f C:\Users\battl\.ssh\id_rsa_vagrant
   ```

**NOTA** Aquí deberéis de usar vuestro usuario, esto es un ejemplo del mio.

   Esto generará dos archivos:
   - `id_rsa_vagrant`: Clave privada (debe mantenerse en secreto).
   - `id_rsa_vagrant.pub`: Clave pública (se usará en Ubuntu Server).

3. **No establecer una frase de contraseña** a menos que desees una capa adicional de seguridad. Si se deja en blanco, podrán acceder sin necesidad de ingresar una contraseña.

4. **Verificar la existencia de los archivos**:
   - Dirígete a la carpeta `C:\Users\battl\.ssh\` y verifica que `id_rsa_vagrant` e `id_rsa_vagrant.pub` estén presentes.

---

### Paso 3: Agregar la Clave Pública Manualmente en la Máquina Virtual Ubuntu Server

Como aún no tienes acceso SSH, este paso se realiza directamente en la consola de Ubuntu Server.

1. **Abrir la clave pública en el sistema Windows**:
   - Abre `id_rsa_vagrant.pub` en PowerShell o Git Bash usando:
     ```powershell
     cat C:\Users\battl\.ssh\id_rsa_vagrant.pub
     ```
   - Copia el contenido completo de la clave pública que se muestra (comienza con `ssh-rsa`).

2. **Agregar la clave pública en Ubuntu Server**:
   - En la consola de Ubuntu Server, crea la carpeta `.ssh` y el archivo `authorized_keys` si no existen:
     ```bash
     mkdir -p ~/.ssh
     nano ~/.ssh/authorized_keys
     ```
   - Pega la clave pública en `authorized_keys` y guarda el archivo (`Ctrl+O` para guardar y `Ctrl+X` para salir en `nano`).

3. **Ajustar permisos en Ubuntu Server**:
   ```bash
   chmod 700 ~/.ssh
   chmod 600 ~/.ssh/authorized_keys
   ```

---

### Paso 4: Probar la Conexión SSH desde Windows Usando la Clave Privada

Una vez que las claves están configuradas, los estudiantes pueden intentar conectarse a la máquina Ubuntu Server desde Windows.

1. **Conectar usando PowerShell o Git Bash**:
   ```powershell
   ssh -i C:\Users\battl\.ssh\id_rsa_vagrant vagrant@192.168.0.45
   ```
   - Reemplaza `vagrant` con el usuario de Ubuntu Server y `192.168.0.45` con la IP estática de la máquina.

2. **Configuración opcional en PuTTY**:
   - Si el estudiante prefiere usar PuTTY, deberán convertir la clave privada al formato PuTTY (.ppk) usando **PuTTYgen**:
     1. Abre **PuTTYgen** y carga `id_rsa_vagrant`.
     2. Guarda la clave como `id_rsa_vagrant.ppk`.
   - Configura PuTTY para usar la clave privada en `SSH > Auth > Private key file for authentication`.

---

### Paso 5: (Opcional) Desactivar la Autenticación por Contraseña

Si quieres que el acceso sea únicamente por clave pública, puedes desactivar la autenticación por contraseña en SSH.

1. **Editar el archivo de configuración SSH en Ubuntu Server**:
   ```bash
   sudo nano /etc/ssh/sshd_config
   ```
2. **Desactivar la autenticación por contraseña**:
   ```plaintext
   PasswordAuthentication no
   ```
3. **Reiniciar el servicio SSH**:
   ```bash
   sudo systemctl restart ssh
   ```

---

### Verificación Final

1. **Comprobar conexión**: Cada estudiante debe asegurarse de que puede conectarse usando la clave privada sin necesidad de contraseña.
2. **Resolver problemas de conexión**:
   - Verificar permisos en el archivo `authorized_keys`.
   - Asegurar que el firewall de Ubuntu permite conexiones SSH:
     ```bash
     sudo ufw allow ssh
     sudo ufw status
     ```
ssh-keygen -R 192.168.56.10