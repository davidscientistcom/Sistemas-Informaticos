### Instalación y Configuración de SSH en Ubuntu Server

### Paso 1: Instalar el Servidor SSH
Para permitir el acceso remoto, primero necesitas instalar el servidor SSH en Ubuntu Server.

1. Actualiza los paquetes del sistema:
   ```bash
   sudo apt update
   ```
2. Instala el paquete de OpenSSH Server:
   ```bash
   sudo apt install openssh-server
   ```

3. Una vez instalado, verifica que el servicio SSH esté activo:
   ```bash
   sudo systemctl status ssh
   ```
   Deberías ver el estado **active (running)**. Si no está activo, inicia el servicio:
   ```bash
   sudo systemctl start ssh
   ```

### Paso 2: Configurar el Servidor SSH
El archivo de configuración principal de SSH es `/etc/ssh/sshd_config`. Puedes modificarlo para personalizar la configuración de SSH.

1. Abre el archivo de configuración:
   ```bash
   sudo nano /etc/ssh/sshd_config
   ```

2. Configura las opciones básicas:
   - **Port 22**: Puerto por defecto para SSH. Puedes cambiarlo a otro puerto para mayor seguridad.
   - **PermitRootLogin no**: Desactiva el acceso root directo para mayor seguridad.
   - **PasswordAuthentication yes**: Asegúrate de que esta opción esté activada si quieres acceder con contraseña.

3. Guarda y cierra el archivo (en nano, `Ctrl+O` para guardar y `Ctrl+X` para salir).

4. Aplica los cambios reiniciando el servicio SSH:
   ```bash
   sudo systemctl restart ssh
   ```

### Paso 3: Configurar el Firewall para Permitir SSH
Si tienes el firewall UFW habilitado, necesitarás permitir el tráfico SSH.

1. Permite conexiones SSH a través del firewall:
   ```bash
   sudo ufw allow ssh
   ```
2. Verifica que el firewall permite SSH:
   ```bash
   sudo ufw status
   ```

---

### Problemas Comunes y Soluciones

#### Problema 1: Error "no hostkeys available -- exiting"
Este error indica que no se han generado las claves de host necesarias para que el servicio SSH funcione.

**Solución:**
1. Genera las claves de host con el siguiente comando:
   ```bash
   sudo ssh-keygen -A
   ```
   Este comando genera las claves de host en el directorio `/etc/ssh/`.

2. Verifica que las claves se hayan creado:
   ```bash
   ls /etc/ssh/ssh_host_*
   ```
   Deberías ver archivos como `ssh_host_rsa_key`, `ssh_host_ecdsa_key`, etc.

3. Reinicia el servicio SSH:
   ```bash
   sudo systemctl restart ssh
   ```

#### Problema 2: Error "Permissions for /etc/ssh/sshd_config are too open"
Este error indica que el archivo de configuración de SSH (`sshd_config`) tiene permisos de acceso demasiado amplios, lo cual es un riesgo de seguridad.

**Solución:**
1. Ajusta los permisos del archivo:
   ```bash
   sudo chmod 600 /etc/ssh/sshd_config
   ```
   Esto limita el acceso al archivo solo para el usuario root.

2. Reinicia el servicio SSH:
   ```bash
   sudo systemctl restart ssh
   ```

#### Problema 3: SSH se Detiene con un Error de Configuración
Si el archivo de configuración contiene errores, SSH no se iniciará correctamente.

**Solución:**
1. Verifica la configuración de SSH antes de reiniciar el servicio:
   ```bash
   sudo sshd -t
   ```
   Este comando muestra cualquier error de sintaxis en el archivo de configuración.

2. Corrige los errores en `/etc/ssh/sshd_config` y vuelve a verificar.

3. Una vez corregido, reinicia SSH:
   ```bash
   sudo systemctl restart ssh
   ```

#### Problema 4: No se Puede Conectar por SSH (Acceso Denegado)
Si no puedes conectarte, podría deberse a la configuración del firewall o a la autenticación.

**Solución:**
1. Verifica que el puerto SSH esté permitido en el firewall:
   ```bash
   sudo ufw allow ssh
   ```

2. Asegúrate de que `PasswordAuthentication` esté configurado en `yes` en `/etc/ssh/sshd_config`.

3. Reinicia el servicio SSH si has cambiado alguna configuración:
   ```bash
   sudo systemctl restart ssh
   ```

---

### Verificar el Acceso por SSH desde Otro Equipo
Para probar la conexión SSH desde tu sistema anfitrión (por ejemplo, Windows), puedes usar **PowerShell** o **PuTTY**:

1. En **PowerShell**, usa:
   ```bash
   ssh nombre_usuario@IP_ESTATICA
   ```
   Reemplaza `nombre_usuario` con tu usuario de Ubuntu Server e `IP_ESTATICA` con la IP configurada.

2. Si usas **PuTTY**:
   - Abre PuTTY, ingresa la IP y el puerto (por defecto, 22).
   - Conéctate y proporciona las credenciales de usuario y contraseña.
