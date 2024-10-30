### 1.3 Clasificación de los Comandos de Linux

Una de las características más importantes de Linux es su enfoque modular, lo que permite dividir los comandos en categorías según su función. Esta clasificación facilita el aprendizaje, ya que los usuarios pueden enfocarse en grupos específicos de comandos dependiendo de sus necesidades.

#### 1.3.1 Comandos de Gestión de Archivos y Directorios

Estos comandos permiten realizar operaciones básicas y avanzadas en el sistema de archivos. Son de los comandos más usados en cualquier sistema operativo y permiten manipular tanto archivos como carpetas de manera eficiente.

- **`ls`**: Lista el contenido de un directorio.
  - **Ejemplo**:
    ```bash
    ls         # Lista archivos y carpetas en el directorio actual
    ls -l      # Muestra detalles como permisos, propietario y tamaño
    ls -a      # Incluye archivos ocultos en la lista
    ```
- **`mkdir`**: Crea un nuevo directorio.
  - **Ejemplo**:
    ```bash
    mkdir nueva_carpeta         # Crea una carpeta llamada "nueva_carpeta"
    mkdir -p dir1/dir2/dir3     # Crea una estructura de carpetas anidadas
    ```
- **`rm`**: Elimina archivos o directorios.
  - **Ejemplo**:
    ```bash
    rm archivo.txt            # Elimina un archivo específico
    rm -r carpeta             # Elimina una carpeta y su contenido de forma recursiva
    rm -rf carpeta_secreta    # Elimina sin pedir confirmación (útil para limpieza rápida)
    ```
- **`cp`**: Copia archivos o directorios.
  - **Ejemplo**:
    ```bash
    cp archivo1.txt archivo2.txt   # Copia "archivo1.txt" como "archivo2.txt"
    cp -r carpeta1 carpeta2        # Copia una carpeta y su contenido
    ```
- **`mv`**: Mueve o renombra archivos y directorios.
  - **Ejemplo**:
    ```bash
    mv archivo1.txt carpeta1/     # Mueve el archivo a una carpeta
    mv archivo1.txt archivo2.txt  # Renombra el archivo
    ```
- **`touch`**: Crea archivos vacíos o actualiza la fecha de modificación de archivos existentes.
  - **Ejemplo**:
    ```bash
    touch nuevo_archivo.txt       # Crea un archivo vacío si no existe
    ```
- **`find`**: Busca archivos y carpetas en el sistema.
  - **Ejemplo**:
    ```bash
    find / -name archivo.txt       # Busca el archivo en el sistema
    find . -type f -name "*.txt"   # Busca archivos con extensión .txt en el directorio actual
    ```

#### 1.3.2 Comandos de Permisos y Propiedades de Archivos

En Linux, la gestión de permisos es fundamental para la seguridad del sistema. Estos comandos permiten ver y modificar permisos, propietarios y grupos de archivos o carpetas.

- **`chmod`**: Cambia los permisos de archivos o carpetas.
  - **Ejemplo**:
    ```bash
    chmod 755 script.sh     # Otorga permisos de lectura, escritura y ejecución al propietario
    chmod u+x archivo.txt   # Agrega permiso de ejecución solo para el usuario propietario
    ```
- **`chown`**: Cambia el propietario de archivos o carpetas.
  - **Ejemplo**:
    ```bash
    chown usuario archivo.txt   # Cambia el propietario del archivo
    chown usuario:grupo archivo.txt  # Cambia el propietario y grupo
    ```
- **`chgrp`**: Cambia el grupo asociado a un archivo o carpeta.
  - **Ejemplo**:
    ```bash
    chgrp grupo archivo.txt     # Cambia el grupo propietario del archivo
    ```

#### 1.3.3 Comandos de Gestión de Usuarios y Grupos

Estos comandos permiten crear, modificar y eliminar usuarios y grupos en el sistema. Su uso es esencial para la administración de sistemas multiusuario.

- **`useradd`**: Crea un nuevo usuario.
  - **Ejemplo**:
    ```bash
    sudo useradd nuevo_usuario       # Crea un nuevo usuario
    sudo useradd -m nuevo_usuario    # Crea un usuario y su directorio personal
    ```
- **`usermod`**: Modifica las propiedades de un usuario.
  - **Ejemplo**:
    ```bash
    sudo usermod -aG grupo usuario   # Agrega al usuario a un grupo
    sudo usermod -d /home/nuevo usuario  # Cambia el directorio personal
    ```
- **`userdel`**: Elimina un usuario del sistema.
  - **Ejemplo**:
    ```bash
    sudo userdel usuario         # Elimina el usuario
    sudo userdel -r usuario      # Elimina el usuario y su directorio personal
    ```
- **`groupadd`**: Crea un nuevo grupo.
  - **Ejemplo**:
    ```bash
    sudo groupadd nuevo_grupo    # Crea un grupo llamado "nuevo_grupo"
    ```
- **`id`**: Muestra la información del usuario actual, incluyendo UID y GID.
  - **Ejemplo**:
    ```bash
    id                           # Información del usuario actual
    id usuario                   # Información de otro usuario
    ```

#### 1.3.4 Comandos de Procesos y Recursos del Sistema

Estos comandos permiten al usuario supervisar y gestionar los procesos y el uso de recursos en el sistema, lo que es crucial para la administración eficiente del sistema.

- **`ps`**: Muestra una lista de procesos en ejecución.
  - **Ejemplo**:
    ```bash
    ps                       # Muestra los procesos en el terminal actual
    ps aux                   # Lista todos los procesos del sistema
    ```
- **`top`**: Visualiza en tiempo real el uso de recursos del sistema.
  - **Ejemplo**:
    ```bash
    top                      # Muestra el uso de CPU, memoria y procesos en tiempo real
    ```
- **`kill`**: Termina un proceso.
  - **Ejemplo**:
    ```bash
    kill 1234                # Termina el proceso con el PID 1234
    kill -9 1234             # Fuerza la terminación del proceso
    ```

#### 1.3.5 Comandos de Red y Conectividad

Para gestionar y monitorear la conectividad en Linux, estos comandos permiten verificar la configuración de red, realizar pruebas de conectividad y transferir archivos.

- **`ifconfig`**: Muestra y configura las interfaces de red.
  - **Ejemplo**:
    ```bash
    ifconfig                  # Muestra la configuración de todas las interfaces
    ifconfig eth0 down        # Desactiva la interfaz eth0
    ifconfig eth0 up          # Activa la interfaz eth0
    ```
- **`ping`**: Verifica la conectividad con una IP o dominio.
  - **Ejemplo**:
    ```bash
    ping google.com           # Envia paquetes ICMP a google.com para verificar conectividad
    ping -c 4 google.com      # Limita el número de paquetes enviados a 4
    ```
- **`scp`**: Copia archivos de manera segura entre sistemas.
  - **Ejemplo**:
    ```bash
    scp archivo.txt usuario@ip_remota:/destino   # Copia un archivo a un servidor remoto
    scp -r carpeta usuario@ip_remota:/destino    # Copia una carpeta a un servidor remoto
    ```

#### 1.3.6 Comandos de Sistema y Administración

Estos comandos son útiles para obtener información del sistema y realizar tareas de administración general, como reiniciar el sistema o ver la fecha actual.

- **`uname`**: Muestra información del sistema.
  - **Ejemplo**:
    ```bash
    uname              # Muestra el nombre del sistema operativo
    uname -a           # Muestra toda la información del sistema
    ```
- **`date`**: Muestra la fecha y hora actual del sistema.
  - **Ejemplo**:
    ```bash
    date               # Muestra la fecha y hora actuales
    date +%Y-%m-%d     # Formatea la salida para mostrar solo la fecha en formato AAAA-MM-DD
    ```
- **`shutdown`**: Apaga o reinicia el sistema.
  - **Ejemplo**:
    ```bash
    sudo shutdown -h now         # Apaga el sistema inmediatamente
    sudo shutdown -r +5          # Reinicia el sistema en 5 minutos
    ```
