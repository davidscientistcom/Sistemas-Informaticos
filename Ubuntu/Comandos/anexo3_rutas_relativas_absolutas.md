## Anexo 3: Rutas Relativas y Absolutas en Linux, Comodines y Uso de `cd`

### 1. Rutas Relativas y Absolutas

En Linux, las rutas (o paths) indican la ubicación de un archivo o directorio en el sistema. Existen dos tipos de rutas: **absolutas** y **relativas**.

#### Rutas Absolutas

Una **ruta absoluta** es la dirección completa de un archivo o carpeta desde la raíz del sistema (`/`). Comienza siempre con una barra diagonal `/`, lo que significa que el sistema empezará a buscar desde el directorio raíz.

**Ejemplos de Rutas Absolutas:**
- `/home/usuario/documentos/informe.txt`: Ruta absoluta hacia un archivo llamado `informe.txt` en la carpeta `documentos` del usuario.
- `/etc/network/interfaces`: Ruta absoluta hacia el archivo de configuración de red.
- `/var/log/syslog`: Ruta absoluta hacia el archivo de registro del sistema.

#### Rutas Relativas

Una **ruta relativa** es la ubicación de un archivo o carpeta en relación al directorio actual (el directorio en el que nos encontramos al ejecutar el comando). No comienza con `/`, sino que indica la posición relativa desde donde estamos.

**Símbolos Comunes en Rutas Relativas:**
- `.` (punto): Representa el **directorio actual**.
- `..` (doble punto): Representa el **directorio padre** (uno arriba en la jerarquía).

**Ejemplos de Rutas Relativas:**
- `./documentos/informe.txt`: Hace referencia al archivo `informe.txt` en la carpeta `documentos` dentro del directorio actual.
- `../proyectos`: Hace referencia al directorio `proyectos`, que se encuentra en el directorio padre del actual.
- `../../config`: Accede a `config`, subiendo dos niveles de directorios.

### 2. Uso de `cd` para Navegar en el Sistema de Archivos

El comando `cd` (cambiar directorio) permite moverse entre directorios en la línea de comandos.

#### Ejemplos Comunes de `cd`

- **Moverse a una Ruta Absoluta**:
  ```bash
  cd /home/usuario/documentos
  ```
  Esto lleva al usuario directamente a la carpeta `documentos` en su directorio personal.

- **Moverse a una Ruta Relativa**:
  ```bash
  cd ../proyectos
  ```
  Mueve el directorio actual un nivel arriba y luego entra en la carpeta `proyectos`.

- **Ir al Directorio Personal (`~`)**:
  ```bash
  cd ~
  ```
  `cd ~` lleva directamente al directorio personal del usuario actual. También se puede usar `cd` sin argumentos para el mismo efecto:
  ```bash
  cd
  ```
  Esto es equivalente a `cd ~`.

- **Regresar al Directorio Anterior**:
  ```bash
  cd -
  ```
  Cambia de nuevo al último directorio en el que estabas antes del actual.

### 3. Comodines para Rutas y Archivos

Los **comodines** son caracteres especiales que permiten hacer coincidir múltiples archivos o directorios de manera flexible. Son especialmente útiles para buscar y manipular archivos que comparten ciertas características en sus nombres.

#### Comodines Comunes

- **Asterisco (`*`)**: Representa **cualquier número de caracteres** (incluyendo ninguno).
  - `ls *.txt`: Lista todos los archivos en el directorio actual que terminan con `.txt`.
  - `cp /home/usuario/*.jpg /backup`: Copia todos los archivos `.jpg` del directorio `/home/usuario` al directorio `/backup`.

- **Interrogación (`?`)**: Representa **un solo carácter**.
  - `ls archivo?.txt`: Coincide con archivos como `archivo1.txt`, `archivo2.txt`, etc., pero no con `archivo12.txt`.

- **Corchetes (`[]`)**: Coinciden con **cualquier carácter dentro de los corchetes**.
  - `ls archivo[1-3].txt`: Coincide con `archivo1.txt`, `archivo2.txt`, y `archivo3.txt`.
  - `ls archivo[ab].txt`: Coincide con `archivoa.txt` y `archivob.txt`.

- **Rangos en Corchetes (`[a-z]`, `[0-9]`)**: Los corchetes también permiten especificar rangos de letras o números.
  - `ls archivo[0-9].txt`: Coincide con cualquier archivo llamado `archivo` seguido de un número del 0 al 9.
  - `ls [A-Z]*`: Lista todos los archivos que comienzan con una letra mayúscula.

### Ejemplos de Uso de Comodines en Rutas y Combinación con Comandos

1. **Listar Todos los Archivos con Extensiones Específicas**  
   ```bash
   ls /home/usuario/*.{jpg,png,gif}
   ```
   Lista todos los archivos de imagen (extensiones `.jpg`, `.png`, y `.gif`) en el directorio `/home/usuario`.

2. **Copiar Archivos con un Formato de Nombre Específico**  
   ```bash
   cp ~/documentos/reporte[1-5].txt ~/backup/
   ```
   Copia cualquier archivo con nombre `reporte1.txt` a `reporte5.txt` en el directorio `documentos` al directorio `backup`.

3. **Eliminar Archivos con Nombres Similares**  
   ```bash
   rm proyecto?.txt
   ```
   Elimina archivos en el directorio actual que tengan un nombre como `proyecto1.txt`, `proyecto2.txt`, etc., pero no `proyecto10.txt`.

4. **Buscar Archivos que Empiecen con una Letra Específica y Terminen en `.conf`**  
   ```bash
   find /etc -name "[abc]*.conf"
   ```
   Busca en `/etc` archivos de configuración (`.conf`) cuyo nombre comience con las letras `a`, `b`, o `c`.

5. **Renombrar Archivos con Comodines y Tuberías**  
   ```bash
   for file in *.txt; do mv "$file" "nuevo_$file"; done
   ```
   Añade el prefijo `nuevo_` a todos los archivos `.txt` en el directorio actual. Este comando es útil cuando necesitas renombrar múltiples archivos de forma simultánea.
