### 1.6 Ejemplos Avanzados y Prácticos de Comandos en Linux

Esta sección proporciona una lista exhaustiva de ejemplos prácticos y casos de uso para todos los comandos explicados anteriormente, incluyendo variaciones y combinaciones con redirecciones y pipelines. El objetivo es que los estudiantes logren una comprensión completa del funcionamiento de cada comando en distintos escenarios.

#### 1.6.1 Gestión de Archivos y Directorios

##### `ls`: Listado de Contenidos en Directorios
El comando `ls` permite explorar los archivos y carpetas en cualquier directorio.

1. **Listar Todos los Archivos en el Directorio Actual, Incluyendo los Ocultos**  
   ```bash
   ls -a
   ```
   Muestra todos los archivos, incluyendo aquellos que comienzan con un punto (`.`), los cuales son archivos ocultos en Linux.

2. **Listar Archivos en Formato Detallado (Permisos, Propietario, Tamaño)**  
   ```bash
   ls -lh /var/log
   ```
   Este comando muestra los archivos en `/var/log` en un formato legible (tamaño en KB, MB, etc.), incluyendo información detallada como permisos, propietario, y tamaño.

##### `mkdir`: Creación de Directorios

1. **Crear un Directorio Simple**  
   ```bash
   mkdir proyectos
   ```
   Crea una carpeta llamada `proyectos` en el directorio actual.

2. **Crear una Estructura Completa de Directorios**  
   ```bash
   mkdir -p proyectos/2024/enero
   ```
   Crea una carpeta llamada `proyectos`, dentro de ella una llamada `2024`, y dentro de esta última, `enero`. Si alguno de estos directorios ya existe, `-p` evita errores.

##### `rm`: Eliminación de Archivos y Directorios

1. **Eliminar un Archivo Específico**  
   ```bash
   rm reporte.txt
   ```
   Elimina el archivo `reporte.txt` en el directorio actual.

2. **Eliminar un Directorio y su Contenido de Forma Recursiva**  
   ```bash
   rm -rf proyectos/2024
   ```
   Borra la carpeta `2024` y todo su contenido, incluyendo archivos y subdirectorios. **Precaución**: La opción `-rf` no pide confirmación.

##### `cp`: Copia de Archivos y Directorios

1. **Copiar un Archivo a Otro Directorio**  
   ```bash
   cp informe.txt /home/usuario/documentos
   ```
   Copia el archivo `informe.txt` al directorio `/home/usuario/documentos`.

2. **Copiar Recursivamente un Directorio Completo**  
   ```bash
   cp -r proyectos backups
   ```
   Copia la carpeta `proyectos` y todo su contenido dentro de la carpeta `backups`. El directorio de destino `backups` debe existir.

##### `mv`: Mover o Renombrar Archivos y Directorios

1. **Renombrar un Archivo**  
   ```bash
   mv documento.txt documento_v1.txt
   ```
   Cambia el nombre de `documento.txt` a `documento_v1.txt`.

2. **Mover un Archivo a Otro Directorio**  
   ```bash
   mv documento_v1.txt /home/usuario/proyectos
   ```
   Mueve el archivo `documento_v1.txt` a la carpeta `/home/usuario/proyectos`.

##### `touch`: Crear Archivos Vacíos

1. **Crear un Archivo Vacío Llamado `nuevo.txt`**  
   ```bash
   touch nuevo.txt
   ```
   Si `nuevo.txt` no existe, se crea; si ya existe, se actualiza su fecha de modificación.

2. **Crear Varios Archivos Vacíos Simultáneamente**  
   ```bash
   touch archivo1.txt archivo2.txt archivo3.txt
   ```
   Este comando crea tres archivos vacíos en el directorio actual.

##### `find`: Búsqueda de Archivos y Directorios

1. **Buscar Archivos con Extensión `.log` en Todo el Sistema**  
   ```bash
   find / -name "*.log"
   ```
   Busca archivos `.log` en todo el sistema. Este comando puede requerir permisos de superusuario.

2. **Buscar y Eliminar Archivos `.tmp` en una Carpeta Específica**  
   ```bash
   find /home/usuario/proyectos -type f -name "*.tmp" -delete
   ```
   Encuentra todos los archivos con extensión `.tmp` en `/home/usuario/proyectos` y los elimina.

#### 1.6.2 Permisos y Propiedades de Archivos

##### `chmod`: Cambio de Permisos

1. **Otorgar Permiso de Ejecución a un Archivo de Script**  
   ```bash
   chmod +x script.sh
   ```
   Añade permisos de ejecución al archivo `script.sh` para todos los usuarios.

2. **Establecer Permisos Exactos para un Archivo**  
   ```bash
   chmod 644 documento.txt
   ```
   Define los permisos de `documento.txt` como: lectura y escritura para el propietario, y solo lectura para grupo y otros.

##### `chown`: Cambio de Propietario de Archivos

1. **Cambiar el Propietario de un Archivo**  
   ```bash
   sudo chown usuario archivo.txt
   ```
   Cambia el propietario de `archivo.txt` a `usuario`.

2. **Cambiar Propietario y Grupo a un Directorio Completo**  
   ```bash
   sudo chown -R usuario:grupo proyectos
   ```
   Cambia el propietario y el grupo de la carpeta `proyectos` y su contenido.

##### `chgrp`: Cambio de Grupo de Archivos

1. **Cambiar el Grupo de un Archivo**  
   ```bash
   chgrp desarrolladores archivo.txt
   ```
   Asigna el archivo `archivo.txt` al grupo `desarrolladores`.

2. **Cambiar el Grupo de Todos los Archivos en una Carpeta**  
   ```bash
   chgrp -R equipo /home/usuario/proyectos
   ```
   Cambia el grupo de la carpeta `/home/usuario/proyectos` y su contenido a `equipo`.

#### 1.6.3 Gestión de Usuarios y Grupos

##### `useradd`, `usermod`, y `userdel`: Administración de Usuarios

1. **Crear un Nuevo Usuario con su Directorio Personal**  
   ```bash
   sudo useradd -m nuevo_usuario
   ```
   Crea el usuario `nuevo_usuario` y genera automáticamente su directorio personal.

2. **Agregar un Usuario a un Grupo Existente**  
   ```bash
   sudo usermod -aG equipo nuevo_usuario
   ```
   Añade `nuevo_usuario` al grupo `equipo`.

##### `groupadd`, `groupmod`, y `groupdel`: Administración de Grupos

1. **Crear un Nuevo Grupo**  
   ```bash
   sudo groupadd proyecto
   ```
   Crea un grupo llamado `proyecto`.

2. **Eliminar un Grupo Existente**  
   ```bash
   sudo groupdel proyecto
   ```
   Elimina el grupo `proyecto` del sistema.

#### 1.6.4 Procesos y Recursos del Sistema

##### `ps`: Monitoreo de Procesos

1. **Ver Procesos Activos del Usuario Actual**  
   ```bash
   ps
   ```
   Lista los procesos en ejecución en la sesión actual.

2. **Ver Todos los Procesos del Sistema con Detalles**  
   ```bash
   ps aux
   ```
   Muestra todos los procesos del sistema con información detallada.

##### `top`: Supervisión en Tiempo Real

1. **Monitoreo Continuo de Procesos y Recursos**  
   ```bash
   top
   ```
   Abre una vista en tiempo real del uso de recursos (CPU, memoria, etc.) y procesos activos.

2. **Filtrar Procesos por Usuario**  
   Dentro de `top`, presiona `u`, luego escribe el nombre de usuario para ver solo los procesos de ese usuario específico.

##### `kill`: Finalización de Procesos

1. **Terminar un Proceso Usando su PID**  
   ```bash
   kill 1234
   ```
   Envía una señal de terminación al proceso con el PID `1234`.

2. **Forzar Terminación de un Proceso**  
   ```bash
   kill -9 1234
   ```
   Envía la señal `SIGKILL`, forzando la finalización del proceso.

#### 1.6.5 Red y Conectividad

##### `ping`: Prueba de Conectividad

1. **Enviar Paquetes de Prueba a una Dirección IP**  
   ```bash
   ping 8.8.8.8
   ```
   Verifica la conectividad con el servidor DNS público de Google.

2. **Limitar el Número de Paquetes Enviados**  
   ```bash
   ping -c 5 google.com
   ```
   Envía solo 5 paquetes a `google.com` para comprobar conectividad.

#### 1.6.6 Comandos de Redirección

##### Redirección de Salida y Entrada

1. **Redirigir la Salida a un Archivo**  
   ```bash
   ls -l > listado

.txt
   ```
   Guarda la salida detallada del comando `ls` en un archivo llamado `listado.txt`.

2. **Agregar Resultados a un Archivo Existente**  
   ```bash
   echo "nueva entrada" >> listado.txt
   ```
   Agrega el texto "nueva entrada" al final del archivo `listado.txt`.

##### Redirección de Entrada

1. **Usar un Archivo como Entrada para un Comando**  
   ```bash
   sort < listado.txt
   ```
   Ordena el contenido de `listado.txt` alfabéticamente y lo muestra en pantalla.

##### Uso de Pipelines para Combinación de Comandos

1. **Buscar un Término Específico en el Historial de Comandos**  
   ```bash
   history | grep "ls"
   ```
   Muestra todas las veces que se ha ejecutado el comando `ls` en el historial.

2. **Obtener el Uso de CPU de los 5 Procesos Más Intensivos**  
   ```bash
   ps aux | sort -nrk 3 | head -5
   ```
   Lista los 5 procesos que más están utilizando la CPU, ordenados de mayor a menor.
