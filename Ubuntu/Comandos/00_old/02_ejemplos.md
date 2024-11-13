### 1.4 Ejemplos Prácticos

Esta sección ofrece una serie de ejercicios prácticos para que los estudiantes puedan aplicar los comandos vistos en situaciones comunes de administración de un sistema Linux. Estos ejemplos les permitirán reforzar sus conocimientos sobre cómo utilizar los comandos de manera efectiva.

#### 1.4.1 Ejercicios de Aplicación Básica

En estos ejercicios, el objetivo es que los estudiantes realicen tareas cotidianas en el sistema de archivos, manipulen permisos y usen comandos de administración básica. Aquí presentamos algunas prácticas para comenzar:

1. **Crear y Organizar Archivos y Directorios**  
   - Crear una carpeta llamada `proyecto` en el directorio personal.
   - Dentro de `proyecto`, crear tres subcarpetas: `documentos`, `imágenes`, y `scripts`.
   - En la carpeta `documentos`, crear tres archivos vacíos llamados `informe.txt`, `resumen.txt`, y `presupuesto.txt` utilizando `touch`.
   - Utilizar `ls -l` en cada carpeta para verificar que se han creado correctamente los archivos y carpetas.
   
   ```bash
   mkdir proyecto
   cd proyecto
   mkdir documentos imágenes scripts
   touch documentos/informe.txt documentos/resumen.txt documentos/presupuesto.txt
   ls -l documentos
   ```

2. **Modificar Permisos y Propietarios**  
   - Cambiar los permisos del archivo `informe.txt` para que solo el usuario propietario tenga permiso de lectura y escritura.
   - Asignar la carpeta `scripts` a un grupo llamado `desarrolladores` y permitir que solo los usuarios del grupo puedan ejecutar los scripts.
   
   ```bash
   chmod 600 documentos/informe.txt
   sudo chgrp desarrolladores scripts
   chmod 750 scripts
   ```

3. **Búsqueda y Filtrado de Archivos**  
   - Buscar todos los archivos `.txt` dentro de la carpeta `proyecto` y sus subcarpetas.
   - Copiar todos los archivos `.txt` encontrados en una nueva carpeta llamada `backups`.
   
   ```bash
   find proyecto -type f -name "*.txt" -exec cp {} backups/ \;
   ```

#### 1.4.2 Ejercicio de Simulación de Administración Básica

Este ejercicio propone una simulación más completa para que los estudiantes practiquen la administración de usuarios, permisos, y organización de archivos en un entorno de trabajo realista.

**Escenario de Ejercicio: Configuración de un Proyecto Compartido**

Un administrador del sistema debe configurar un entorno de trabajo para un equipo de desarrollo. La estructura del proyecto tiene los siguientes requisitos:
- Crear usuarios llamados `usuario1`, `usuario2`, y `usuario3`.
- Crear un grupo llamado `equipo`.
- Agregar los usuarios al grupo `equipo`.
- Crear una carpeta `proyecto_compartido` en `/home`.
- Cambiar el propietario de `proyecto_compartido` para que pertenezca al grupo `equipo`.
- Asignar permisos de lectura, escritura y ejecución solo para los miembros de `equipo`.
  
**Solución Paso a Paso:**

1. **Crear Usuarios y Grupo**  
   ```bash
   sudo useradd usuario1
   sudo useradd usuario2
   sudo useradd usuario3
   sudo groupadd equipo
   sudo usermod -aG equipo usuario1
   sudo usermod -aG equipo usuario2
   sudo usermod -aG equipo usuario3
   ```

2. **Crear la Carpeta y Asignar Permisos**  
   ```bash
   sudo mkdir /home/proyecto_compartido
   sudo chown :equipo /home/proyecto_compartido
   sudo chmod 770 /home/proyecto_compartido
   ```

3. **Verificar la Configuración**  
   - Listar los permisos de la carpeta para confirmar que los usuarios del grupo `equipo` tienen acceso:
   ```bash
   ls -ld /home/proyecto_compartido
   ```

Este ejercicio permitirá a los estudiantes practicar la administración de usuarios, grupos, y permisos, así como entender cómo configurar un entorno de trabajo colaborativo en un sistema Linux.

### 1.5 Consejos Prácticos para el Uso de Comandos en Linux

Para maximizar la eficiencia en el uso de la línea de comandos, es fundamental conocer ciertos trucos y convenciones en Linux. A continuación, algunos consejos que ayudarán a los estudiantes a trabajar de manera más rápida y eficiente.

#### 1.5.1 Uso de Combinaciones de Comandos (Pipelines)

En Linux, es posible combinar comandos para realizar varias operaciones en secuencia sin almacenar resultados intermedios. Este proceso se llama “pipeline” y se representa con el operador `|`.

**Ejemplo de Uso de Pipeline:**
```bash
ls -l /etc | grep ".conf"
```
En este caso, `ls -l /etc` lista todos los archivos y carpetas en `/etc`, y `grep ".conf"` filtra solo aquellos archivos que terminan en `.conf`.

Otro ejemplo:
```bash
ps aux | sort -nrk 3 | head -5
```
Este comando muestra los 5 procesos que más CPU están consumiendo. Primero, `ps aux` muestra todos los procesos en ejecución; luego, `sort -nrk 3` los ordena de forma descendente según el uso de CPU (columna 3); finalmente, `head -5` muestra solo los primeros cinco resultados.

#### 1.5.2 Redirección de Salida y Entrada

La redirección permite enviar la salida de un comando a un archivo o usar un archivo como entrada en un comando. Esto es útil para almacenar resultados o procesar datos de archivos.

**Operadores de Redirección Comunes:**
- `>`: Redirige la salida a un archivo, reemplazando su contenido si ya existe.
- `>>`: Redirige la salida a un archivo, agregando contenido si ya existe.
- `<`: Utiliza un archivo como entrada para un comando.

**Ejemplos de Redirección:**
```bash
echo "Hola, mundo" > saludo.txt    # Crea o reemplaza el archivo saludo.txt con el mensaje
echo "Añadiendo más texto" >> saludo.txt  # Añade el texto al archivo sin reemplazarlo
```

Otro ejemplo con redirección de entrada:
```bash
sort < lista_nombres.txt
```
Este comando ordena alfabéticamente el contenido de `lista_nombres.txt` y muestra el resultado en pantalla.

> **Consejo Avanzado**: La redirección puede combinarse con pipelines para crear comandos complejos. Por ejemplo:
> ```bash
> grep "error" /var/log/syslog | sort | uniq > errores_unicos.txt
> ```
> Este comando busca la palabra “error” en el archivo `syslog`, ordena los resultados, elimina duplicados con `uniq` y guarda la salida en `errores_unicos.txt`.
