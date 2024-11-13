# Guía Práctica de Comandos en Ubuntu: `mkdir`, `rm` y Renombrado de Archivos y Directorios

### Comando `mkdir` (Make Directory)

#### Ejercicio 1: Crear un Directorio Sencillo
**Enunciado:** Crea un directorio llamado `proyectos` en tu directorio de inicio.

1. **Comando:**  
   ```bash
   mkdir ~/proyectos
   ```
   **Explicación:** Esto crea el directorio `proyectos` en la ruta `/home/tu_usuario/proyectos` (ruta absoluta).

#### Ejercicio 2: Crear un Directorio en una Ruta Relativa
**Enunciado:** Navega a la carpeta `proyectos` que has creado y dentro de ella crea un directorio llamado `practicas`.

1. **Comandos:**
   ```bash
   cd ~/proyectos
   mkdir practicas
   ```
   **Explicación:** Aquí primero usamos la ruta relativa `cd` para acceder a `proyectos` y luego creamos `practicas` dentro de ella.

#### Ejercicio 3: Crear Múltiples Directorios Simultáneamente
**Enunciado:** Crea, dentro de `practicas`, las siguientes carpetas: `python`, `bash` y `networking`.

1. **Comando:**
   ```bash
   mkdir ~/proyectos/practicas/{python,bash,networking}
   ```
   **Explicación:** Este comando usa llaves `{}` para crear múltiples carpetas al mismo tiempo.

#### Ejercicio 4: Crear Directorios con Subdirectorios
**Enunciado:** Dentro de `python`, crea una estructura de carpetas con una sola línea de comando: `ejercicios` y dentro de esta, `basico` y `avanzado`.

1. **Comando:**
   ```bash
   mkdir -p ~/proyectos/practicas/python/ejercicios/{basico,avanzado}
   ```
   **Explicación:** La opción `-p` permite crear subdirectorios automáticamente, aunque las carpetas superiores no existan.

---

### Comando `rm` (Remove)

#### Ejercicio 5: Eliminar un Archivo Específico
**Enunciado:** Crea un archivo vacío dentro de `bash` llamado `temp.txt` y luego elimínalo usando el comando `rm`.

1. **Comando:**
   ```bash
   touch ~/proyectos/practicas/bash/temp.txt
   rm ~/proyectos/practicas/bash/temp.txt
   ```
   **Explicación:** `rm` elimina el archivo `temp.txt`. Ten en cuenta que este proceso no es reversible.

#### Ejercicio 6: Eliminar un Directorio Vacío
**Enunciado:** Elimina el directorio `networking` que creaste previamente.

1. **Comando:**
   ```bash
   rm -d ~/proyectos/practicas/networking
   ```
   **Explicación:** La opción `-d` elimina un directorio vacío.

#### Ejercicio 7: Eliminar un Directorio con Contenido
**Enunciado:** Crea un archivo `old_code.py` en `python/ejercicios/basico` y elimina el directorio `basico` junto con su contenido.

1. **Comandos:**
   ```bash
   touch ~/proyectos/practicas/python/ejercicios/basico/old_code.py
   rm -r ~/proyectos/practicas/python/ejercicios/basico
   ```
   **Explicación:** La opción `-r` permite borrar recursivamente, eliminando también los archivos y subdirectorios.

---

### Renombrar Archivos y Directorios

#### Ejercicio 8: Renombrar un Archivo
**Enunciado:** Crea un archivo llamado `proyecto_final.txt` en `proyectos` y cámbiale el nombre a `proyecto_completo.txt`.

1. **Comandos:**
   ```bash
   touch ~/proyectos/proyecto_final.txt
   mv ~/proyectos/proyecto_final.txt ~/proyectos/proyecto_completo.txt
   ```
   **Explicación:** El comando `mv` se usa para renombrar archivos.

#### Ejercicio 9: Renombrar un Directorio
**Enunciado:** Cambia el nombre del directorio `python` a `scripts_python`.

1. **Comando:**
   ```bash
   mv ~/proyectos/practicas/python ~/proyectos/practicas/scripts_python
   ```
   **Explicación:** También puedes usar `mv` para renombrar directorios.

#### Ejercicio 10: Mover y Renombrar un Archivo a la Vez
**Enunciado:** Mueve el archivo `proyecto_completo.txt` a la carpeta `bash` y renómbralo como `final_project.txt`.

1. **Comando:**
   ```bash
   mv ~/proyectos/proyecto_completo.txt ~/proyectos/practicas/bash/final_project.txt
   ```
   **Explicación:** `mv` puede mover y renombrar un archivo en una misma operación.
