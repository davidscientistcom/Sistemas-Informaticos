## Configuración Completa de SSH con Clave Pública desde Cero

Este proceso detalla cómo configurar el acceso SSH en una máquina **Ubuntu Server** en **VirtualBox** con autenticación por clave desde un sistema Windows, sin acceso inicial por SSH.

### Objetivo
Poder conectarnos desde windows  a una máquina virtual Ubuntu Server configurada en VirtualBox, utilizando autenticación por clave SSH.

### Prerrequisitos
- **Máquina virtual Ubuntu Server** instalada en **VirtualBox**.
- **IP estática configurada** en la máquina virtual para que sea accesible desde el host.



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

### Paso 2: Testear la conexión 

La forma más sencilla es sabiendo la ip, por ejemplo, si la máquina tiene una IP: 192.168.1.20 y el usuario es david:
```bash
   ssh david@192.168.1.20,

``` 

### Paso : Fingerprint en ssh
Voy a explicarte detalladamente qué significan esos mensajes de "fingerprint" que ves al conectarte por SSH, así como el papel que juega el archivo `authorized_keys` y por qué en ocasiones es necesario borrar o modificar entradas en él.


### ¿Qué es el fingerprint?

Cuando te conectas por SSH a un servidor por primera vez, el cliente SSH (tu ordenador) recibe la clave pública del servidor. Para no mostrar la clave completa (que puede ser bastante larga), se calcula un **fingerprint** o huella digital. Este fingerprint es, básicamente, un resumen (hash) de la clave pública del servidor.

### ¿Para qué sirve?

- **Verificación de la identidad del servidor:**  
  El fingerprint te permite confirmar que el servidor al que te estás conectando es realmente el que esperas. La primera vez que te conectas, el cliente te muestra el fingerprint y te pregunta si confías en él. Si lo aceptas, se guarda en tu archivo `known_hosts`.

- **Protección contra ataques Man-in-the-Middle:**  
  Si alguien intenta interceptar o suplantar el servidor, la clave pública (y, por ende, su fingerprint) será diferente. En una conexión posterior, el cliente SSH detectará la diferencia y te avisará de un posible problema.

### Ejemplo en la práctica

Cuando te conectas a un servidor nuevo, podrías ver un mensaje como:

```plaintext
The authenticity of host '192.168.1.100 (192.168.1.100)' can't be established.
ECDSA key fingerprint is SHA256:abc123def456ghi789...
Are you sure you want to continue connecting (yes/no)?
```

- **Qué ocurre en ese momento:**  
  El cliente SSH muestra el fingerprint calculado a partir de la clave pública del servidor. Si tú verificas (por ejemplo, comparándolo con un fingerprint proporcionado por el administrador del servidor) y confirmas que coincide, puedes responder "yes". Una vez aceptado, el fingerprint se almacena en el archivo `~/.ssh/known_hosts` de tu máquina local, y en conexiones futuras, el cliente lo compara para asegurarse de que el servidor no haya cambiado su clave.



## 2. **El archivo `authorized_keys`**

### ¿Qué es y para qué sirve?

El archivo `authorized_keys` es un archivo que se encuentra en el directorio `~/.ssh/` del usuario en el servidor (por ejemplo, `/home/usuario/.ssh/authorized_keys`). Este archivo contiene una lista de **claves públicas** autorizadas para acceder a ese usuario mediante SSH.

**Funcionamiento:**

- **Autenticación mediante clave pública:**  
  Cuando intentas conectarte por SSH usando autenticación por clave, el servidor compara la clave pública que envías con las que tiene registradas en `authorized_keys`. Si encuentra una coincidencia, te permite la conexión sin necesidad de pedirte una contraseña.

- **Control de acceso:**  
  Puedes añadir o quitar claves en este archivo para controlar qué dispositivos o usuarios pueden acceder al servidor. Cada línea del archivo representa una clave autorizada.

### Ejemplo de contenido de `authorized_keys`

```plaintext
ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAACAQ... clave_maquina_virtual
ssh-ed25519 AAAAC3NzaC1lZDI1NTE5AAAAIF... otra_clave_para_otro_equipo
```

En este ejemplo, cada línea es una clave pública asociada a algún dispositivo o usuario, junto con un comentario (al final) que ayuda a identificar la clave.



## 3. **¿Por qué a veces es necesario borrar o modificar entradas en `authorized_keys`?**

Hay varias razones por las cuales podrías necesitar limpiar o modificar este archivo:

1. **Cambio o reemplazo de claves:**  
   Si un dispositivo cambia su par de claves (por ejemplo, por cuestiones de seguridad o porque se perdió el acceso a la clave anterior), tendrás que eliminar la entrada antigua y agregar la nueva.

2. **Eliminación de accesos no deseados:**  
   Si un usuario o dispositivo ya no debería tener acceso al servidor, se debe eliminar su clave del archivo `authorized_keys`.

3. **Errores o duplicados:**  
   A veces, al copiar claves manualmente, pueden generarse entradas duplicadas o errores en el archivo que impidan la correcta autenticación. Limpiar el archivo y dejar solo las entradas válidas es una buena práctica para evitar conflictos.

4. **Mantenimiento de seguridad:**  
   Con el tiempo, la lista de claves autorizadas puede crecer. Revisarla periódicamente te ayuda a asegurarte de que solo las claves necesarias y actuales se mantengan, reduciendo la superficie de ataque.

### Ejemplo práctico: Borrar una entrada

Si, por ejemplo, ya no confías en la clave asociada al comentario `clave_antigua`, puedes conectarte al servidor y editar el archivo:

```bash
ssh usuario@ip_del_servidor
nano ~/.ssh/authorized_keys
```

Luego, localizas la línea correspondiente a `clave_antigua` y la eliminas. Tras guardar los cambios, esa clave ya no permitirá el acceso.

A continuación, te explico de forma detallada y paso a paso cómo copiar tu clave pública al servidor Ubuntu que tienes en VirtualBox para que puedas acceder mediante SSH sin que te solicite contraseña. Imagina que estamos escribiendo un manual de texto para que los chavales puedan seguir cada paso con claridad.

# Certificados en el servidor.

## 1. **Generar el par de claves (si aún no lo has hecho)**

Si ya tienes generado tu par de claves (clave privada y clave pública), puedes saltarte este paso. En caso contrario, en tu máquina local (desde donde te conectas) abre una terminal y ejecuta:

```bash
ssh-keygen -t rsa -b 4096 -C "tu_correo@ejemplo.com"
```

**Explicación:**

- **`-t rsa`**: Especifica que quieres generar una clave RSA.
- **`-b 4096`**: Indica que la longitud de la clave será de 4096 bits, lo que mejora la seguridad.
- **`-C "tu_correo@ejemplo.com"`**: Es un comentario que ayuda a identificar la clave (por ejemplo, con tu correo).

Durante el proceso se te preguntará dónde guardar la clave. La ruta por defecto es `~/.ssh/id_rsa` para la clave privada y `~/.ssh/id_rsa.pub` para la clave pública. También se te pedirá que introduzcas una frase de contraseña (passphrase) para mayor seguridad; si prefieres no utilizar una, simplemente pulsa Enter.

--- 
Si en lugar del fichero, y **es mi forma recomendada** lo que quiero es poner un nombre al certificado para que todos los certificados que yo tenga tengan un nombre claro, deberíamos de hacerlo con:

```bash
ssh-keygen -t rsa -b 4096 -C "nombre_concreto"
```



## 2. **Copiar la clave pública al servidor Ubuntu**

La idea es que el servidor tenga una copia de tu clave pública para reconocer y autorizar tus conexiones sin necesidad de introducir una contraseña cada vez.

### **Opción A: Usando `ssh-copy-id`**

Esta herramienta simplifica mucho el proceso. En tu máquina local, ejecuta:

```bash
ssh-copy-id usuario@ip_maquina_virtual
```

**Donde:**

- **`usuario`**: Es el nombre de usuario en el servidor Ubuntu.
- **`ip_maquina_virtual`**: Es la dirección IP (o nombre de host) de la máquina virtual.

**Qué hace este comando:**

- Se conecta al servidor mediante SSH.
- Crea el directorio `~/.ssh` en el servidor (si no existe).
- Añade tu clave pública al archivo `~/.ssh/authorized_keys` de forma segura, asegurándose de no sobrescribir entradas existentes.

### **Opción B: Copiar manualmente la clave pública**

Si por alguna razón no cuentas con `ssh-copy-id` o prefieres hacerlo manualmente, sigue estos pasos:

1. En tu máquina local, visualiza el contenido de tu clave pública:

    ```bash
    cat ~/.ssh/id_rsa.pub
    ```

2. Copia el resultado (todo el contenido de esa línea).

3. Conéctate al servidor Ubuntu (inicialmente con contraseña):

    ```bash
    ssh usuario@ip_maquina_virtual
    ```

4. Una vez conectado, asegúrate de tener el directorio `.ssh` creado y con los permisos correctos:

    ```bash
    mkdir -p ~/.ssh
    chmod 700 ~/.ssh
    ```

5. Edita (o crea) el archivo `authorized_keys` en el directorio `~/.ssh`:

    ```bash
    nano ~/.ssh/authorized_keys
    ```

6. Pega la clave pública que copiaste en el archivo y guarda los cambios.

7. Asegúrate de que el archivo tenga los permisos correctos:

    ```bash
    chmod 600 ~/.ssh/authorized_keys
    ```

8. Sal del servidor:

    ```bash
    exit
    ```

**Nota:** Otra forma, si prefieres hacerlo todo desde la máquina local sin abrir un editor en el servidor, es ejecutar:

```bash
cat ~/.ssh/id_rsa.pub | ssh usuario@ip_maquina_virtual "mkdir -p ~/.ssh && chmod 700 ~/.ssh && cat >> ~/.ssh/authorized_keys && chmod 600 ~/.ssh/authorized_keys"
```



## 3. **Verificar los permisos y la configuración en el servidor**

Es fundamental que el directorio `~/.ssh` y el archivo `authorized_keys` tengan los permisos adecuados para que el servidor SSH funcione correctamente:

- **Directorio `~/.ssh`**: Permisos **700** (lectura, escritura y ejecución solo para el propietario).

    ```bash
    chmod 700 ~/.ssh
    ```

- **Archivo `authorized_keys`**: Permisos **600** (lectura y escritura solo para el propietario).

    ```bash
    chmod 600 ~/.ssh/authorized_keys
    ```

Si estos permisos son demasiado permisivos, el servidor SSH puede negarse a utilizar el archivo por motivos de seguridad.



## 4. **Probar la conexión sin contraseña**

Con todo configurado, prueba conectarte nuevamente desde tu máquina local:

```bash
ssh usuario@ip_maquina_virtual
```

Si todo está correcto, no se te pedirá la contraseña, y el proceso de autenticación se realizará mediante el certificado (clave pública y privada).



## 5. **Solución de problemas comunes**

Si por algún motivo la conexión aún te solicita contraseña, revisa lo siguiente:

1. **Verifica la ubicación y el nombre de las claves:**  
   Asegúrate de que la clave pública que copiaste es la correcta y se encuentra en `~/.ssh/id_rsa.pub` (o el nombre que hayas especificado al generarla).

2. **Revisa los permisos:**  
   Tanto en el directorio `~/.ssh` como en `authorized_keys`, los permisos deben ser estrictos (700 y 600, respectivamente).

3. **Configuración del servidor SSH:**  
   Revisa el archivo de configuración `/etc/ssh/sshd_config` en el servidor Ubuntu para asegurarte de que las siguientes líneas no estén deshabilitando la autenticación mediante clave pública:

   ```plaintext
   PubkeyAuthentication yes
   AuthorizedKeysFile    .ssh/authorized_keys
   ```

   Después de cualquier cambio en este archivo, recuerda reiniciar el servicio SSH:

   ```bash
   sudo systemctl restart ssh
   ```

4. **Logs de SSH:**  
   Si sigues teniendo problemas, revisa los logs del servidor para obtener más detalles:

   ```bash
   sudo tail -f /var/log/auth.log
   ```