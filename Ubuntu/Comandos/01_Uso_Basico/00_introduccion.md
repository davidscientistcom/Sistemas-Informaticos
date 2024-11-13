### Capítulo 1: Introducción a los Comandos de Linux

#### 1.1 Conceptos Fundamentales

##### 1.1.1 Qué es un Comando en Linux

**Definición de Comando**  
En el contexto de Linux, un comando es una instrucción o solicitud que se le da al sistema operativo a través de la línea de comandos para realizar una acción específica. Los comandos son el medio por el cual el usuario puede interactuar directamente con el sistema operativo, solicitando desde operaciones simples (como listar el contenido de una carpeta) hasta tareas avanzadas (como manipular permisos de archivos o gestionar procesos).

**Comandos Internos y Externos**  
- **Comandos Internos**: Son aquellos que forman parte del propio intérprete de comandos (o shell), como `cd`, `echo` o `exit`. Estos comandos no necesitan acceder a un programa externo, ya que el shell puede ejecutarlos por sí mismo.
- **Comandos Externos**: Son ejecutables que residen en el sistema de archivos y están disponibles en ubicaciones específicas (normalmente definidas en la variable de entorno `PATH`). Algunos ejemplos son `ls`, `cp`, `grep`, que dependen de un archivo ejecutable ubicado en el sistema.

##### Ejemplo de Comandos Internos y Externos
Para saber si un comando es interno o externo, puedes usar `type` seguido del comando que desees consultar:
```bash
type cd   # Muestra que es un comando interno del shell
type ls   # Muestra la ubicación del comando, indicando que es externo
```

##### 1.1.2 Estructura de un Comando

La sintaxis básica de un comando en Linux sigue el formato:
```bash
comando [opciones] [argumentos]
```
A continuación, se desglosan estos componentes:

**1. Comando**  
Es la acción principal que se desea ejecutar. Por ejemplo, `ls` se utiliza para listar el contenido de un directorio, mientras que `rm` se utiliza para eliminar archivos o directorios.

**2. Opciones**  
Las opciones permiten modificar el comportamiento de un comando para ajustar el resultado a nuestras necesidades. Normalmente, se preceden por un guion simple (`-`) o doble (`--`). Por ejemplo, `-l` en `ls -l` muestra los detalles de los archivos en un formato largo (long listing).

   - **Ejemplos**:
     - `ls -a`: Muestra todos los archivos, incluyendo los ocultos.
     - `ls -l -h`: Muestra los detalles en formato largo y con tamaño legible para humanos.
     - `grep --color`: Resalta el texto coincidente en el resultado de la búsqueda.

**3. Argumentos**  
Los argumentos son la información adicional que el comando necesita para realizar la acción. Pueden ser nombres de archivos, carpetas, patrones de búsqueda, etc. Por ejemplo, `ls /home/user/Documents` utiliza `/home/user/Documents` como argumento, indicando la carpeta específica cuyo contenido debe listar el comando `ls`.

##### Ejemplo Completo
Veamos el siguiente comando como ejemplo:
```bash
ls -lh /var/log
```
En este caso:
- `ls` es el comando principal, que lista los archivos y directorios.
- `-lh` son las opciones: `-l` activa el formato largo, mientras que `-h` convierte el tamaño de los archivos a un formato legible (KB, MB).
- `/var/log` es el argumento, indicando el directorio específico a listar.
