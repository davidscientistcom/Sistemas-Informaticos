### SSH: Secure Shell

El **Secure Shell** o **SSH** es un protocolo de red que permite conectarse de forma segura a otro ordenador o dispositivo en la red mediante una interfaz de línea de comandos. SSH se usa principalmente para administrar servidores y dispositivos remotos, proporcionando un canal seguro sobre redes inseguras. Es una herramienta esencial para administradores de sistemas y desarrolladores que necesitan acceder remotamente a máquinas Linux, servidores o cualquier dispositivo que permita una conexión SSH.

#### Funcionamiento de SSH

SSH utiliza un modelo cliente-servidor, donde el cliente (el dispositivo desde el cual se establece la conexión) inicia una solicitud de conexión hacia el servidor (el dispositivo al que se quiere acceder). La conexión es cifrada, lo que significa que los datos transmitidos están protegidos contra la interceptación y manipulación por terceros.

SSH emplea autenticación basada en claves públicas o contraseñas. Las claves SSH ofrecen mayor seguridad y consisten en un par de claves:
- **Clave pública**: Se almacena en el servidor al que se quiere acceder.
- **Clave privada**: Permanece en el cliente y nunca debe compartirse.

#### Ejemplo de Uso de SSH

Para conectar un cliente a un servidor mediante SSH, es necesario tener:
- **Dirección IP o nombre de dominio del servidor**.
- **Usuario y contraseña** (o clave pública previamente configurada en el servidor para autenticación sin contraseña).

Supón que tienes un servidor con IP `192.168.1.10` y un usuario llamado `usuario`.

1. **Conexión básica con contraseña**:

   Desde el cliente, en una terminal, puedes conectarte al servidor de la siguiente manera:

   ```bash
   ssh usuario@192.168.1.10
   ```

   Al ejecutarse el comando, SSH solicitará la contraseña del usuario en el servidor. Tras ingresarla correctamente, se establece la conexión y podrás interactuar con el servidor como si estuvieras directamente frente a él.

2. **Conexión con clave pública**:

   Para aumentar la seguridad, es preferible usar autenticación mediante claves en lugar de contraseñas. Este método implica generar un par de claves (pública y privada) en el cliente y configurar el servidor para que reconozca la clave pública.

   - **Generar un par de claves**:

     En el cliente, ejecuta:

     ```bash
     ssh-keygen -t rsa -b 2048
     ```

     Este comando generará dos archivos:
     - `~/.ssh/id_rsa` (clave privada)
     - `~/.ssh/id_rsa.pub` (clave pública)

   - **Copiar la clave pública al servidor**:

     Una vez generadas las claves, copia la clave pública al servidor usando el siguiente comando:

     ```bash
     ssh-copy-id usuario@192.168.1.10
     ```

     Esto configurará automáticamente la clave pública en el servidor, permitiendo la autenticación sin contraseña en futuras conexiones.

   - **Conectarse sin contraseña**:

     Ahora, simplemente ejecuta:

     ```bash
     ssh usuario@192.168.1.10
     ```

     Dado que el servidor reconoce la clave pública del cliente, se establecerá la conexión sin necesidad de ingresar una contraseña.

#### Configuración de SSH en un Servidor Linux

Si el servidor no tiene SSH activado, estos son los pasos para configurarlo en una máquina Linux:

1. **Instalar el servidor SSH** (en el servidor):

   ```bash
   sudo apt update
   sudo apt install openssh-server
   ```

2. **Iniciar y habilitar el servicio SSH**:

   ```bash
   sudo systemctl start ssh
   sudo systemctl enable ssh
   ```

3. **Configurar el archivo `sshd_config`** (archivo de configuración principal para el servidor SSH), ubicado en `/etc/ssh/sshd_config`, donde se pueden ajustar aspectos como el puerto, el acceso root y los métodos de autenticación.

4. **Reiniciar el servicio SSH** después de realizar cambios en la configuración:

   ```bash
   sudo systemctl restart ssh
   ```

#### Opciones Comunes del Comando SSH

- **Especificar puerto**: Por defecto, SSH usa el puerto 22, pero si el servidor está configurado en un puerto diferente, puedes especificarlo usando la opción `-p`:

  ```bash
  ssh -p 2222 usuario@192.168.1.10
  ```

- **Ejecutar un solo comando remoto**: Es posible ejecutar un comando específico en el servidor sin iniciar una sesión completa. Esto resulta útil para tareas de administración rápida:

  ```bash
  ssh usuario@192.168.1.10 "ls -l /var/www"
  ```

- **Redirección de puertos (Port Forwarding)**: SSH permite redirigir puertos locales y remotos, proporcionando un túnel seguro para el tráfico. Esto se usa a menudo para acceder a servicios privados dentro de una red.

  ```bash
  ssh -L 8080:localhost:80 usuario@192.168.1.10
  ```

  En este ejemplo, el puerto local 8080 se redirige al puerto 80 del servidor.

#### Ejemplo Práctico de Uso de SSH

Imaginemos que necesitas acceder a un servidor de la oficina para revisar archivos. Tienes configurada la clave pública en el servidor y conoces la IP del mismo. Para conectarte, abre una terminal y ejecuta:

```bash
ssh usuario@192.168.1.10
```

Ahora tienes acceso al servidor y puedes realizar tareas como verificar el estado de servicios, consultar archivos de registro o instalar actualizaciones.

#### Ventajas de SSH

- **Seguridad**: SSH ofrece cifrado y autenticación segura, protegiendo la integridad y confidencialidad de la información transmitida.
- **Versatilidad**: Permite administrar de forma remota servidores y dispositivos, transferir archivos, ejecutar comandos remotos y crear túneles seguros.
- **Facilidad de uso**: Con una configuración inicial de clave pública, SSH permite conexiones rápidas y seguras sin necesidad de ingresar contraseñas cada vez.

#### Transferencia de Archivos con SSH

SSH también permite transferir archivos de forma segura usando **SCP** (Secure Copy) o **SFTP** (Secure File Transfer Protocol).

- **SCP**: Usado para copiar archivos entre el cliente y el servidor:

  ```bash
  scp archivo.txt usuario@192.168.1.10:/ruta/destino/
  ```

- **SFTP**: Protocolo interactivo similar a FTP, pero seguro. Se accede con:

  ```bash
  sftp usuario@192.168.1.10
  ```