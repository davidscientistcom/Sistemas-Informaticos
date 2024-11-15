### Capítulo 3: Trabajando con el Sistema de Archivos

#### Objetivo del Capítulo
Aprenderás a usar Python para gestionar el sistema de archivos: crear, renombrar, mover y eliminar archivos y carpetas. Esto te permitirá automatizar tareas de organización de archivos, una habilidad clave en la administración de sistemas.

---

### 3.1 Introducción al Módulo `os`

Python ofrece el módulo `os` para interactuar con el sistema de archivos. Con este módulo, podrás acceder a funciones que permiten crear y gestionar archivos y carpetas, así como navegar por el sistema de archivos.

**1. ¿Qué es el módulo `os`?**
   - El módulo `os` permite a Python realizar tareas en el sistema operativo, como manipular archivos y directorios.
   - Antes de utilizarlo, asegúrate de importarlo en tu código:
     ```python
     import os
     ```

**2. Ejercicio: Listar Archivos en el Directorio Actual**
   - **Objetivo**: Escribir un script que muestre todos los archivos y carpetas en el directorio actual.
   - **Paso 1**: Crea un archivo `listar_archivos.py` y escribe el siguiente código:
     ```python
     import os

     print("Archivos y carpetas en el directorio actual:")
     for item in os.listdir("."):
         print(item)
     ```
   - **Paso 2**: Guarda y ejecuta el script. Observa cómo el programa muestra todos los archivos y carpetas en el directorio.

---

### 3.2 Crear y Eliminar Directorios

Para administrar archivos y carpetas, es esencial saber crear y eliminar directorios.

**1. Crear Directorios**
   - Usando `os.mkdir()` puedes crear un nuevo directorio.
   - **Ejemplo**:
     ```python
     import os

     os.mkdir("nueva_carpeta")
     print("Directorio 'nueva_carpeta' creado.")
     ```
   - **Ejercicio**: Crea un archivo `crear_directorio.py` y escribe el código anterior. Ejecútalo y verifica que el directorio fue creado.

**2. Eliminar Directorios**
   - Para eliminar un directorio vacío, usa `os.rmdir()`.
   - **Ejemplo**:
     ```python
     import os

     os.rmdir("nueva_carpeta")
     print("Directorio 'nueva_carpeta' eliminado.")
     ```
   - **Ejercicio**: Añade el código en un archivo `eliminar_directorio.py` y ejecútalo después de crear `nueva_carpeta`. Verifica que el directorio fue eliminado.

**Nota**: `os.rmdir()` solo elimina directorios vacíos. Más adelante veremos cómo eliminar directorios con contenido.

---

### 3.3 Crear y Renombrar Archivos

**1. Crear un Archivo**
   - Puedes crear archivos directamente en Python usando el método `open()`.
   - **Ejemplo**:
     ```python
     archivo = open("archivo_ejemplo.txt", "w")
     archivo.write("Este es un archivo de ejemplo.")
     archivo.close()
     print("Archivo 'archivo_ejemplo.txt' creado.")
     ```
   - **Ejercicio**: Guarda el código en un archivo llamado `crear_archivo.py`, ejecútalo y revisa que el archivo `archivo_ejemplo.txt` se haya creado.

**2. Renombrar Archivos**
   - Usa `os.rename()` para cambiar el nombre de un archivo.
   - **Ejemplo**:
     ```python
     import os

     os.rename("archivo_ejemplo.txt", "archivo_renombrado.txt")
     print("Archivo renombrado a 'archivo_renombrado.txt'.")
     ```
   - **Ejercicio**: Crea un archivo `renombrar_archivo.py` con este código y ejecútalo. Verifica que el archivo se haya renombrado.

---

### 3.4 Eliminar Archivos

Para eliminar archivos, Python proporciona el método `os.remove()`.

**Ejemplo de Eliminación de Archivos**
   ```python
   import os

   os.remove("archivo_renombrado.txt")
   print("Archivo 'archivo_renombrado.txt' eliminado.")
   ```
   
**Ejercicio**
   - Crea un archivo `eliminar_archivo.py`, escribe el código y ejecútalo para eliminar el archivo `archivo_renombrado.txt`. Verifica que el archivo haya desaparecido del directorio.

**Nota**: Usa esta función con precaución, ya que eliminará el archivo de forma permanente.

---

### 3.5 Navegar entre Directorios

**1. Cambiar de Directorio**
   - Con `os.chdir()`, puedes cambiar el directorio actual.
   - **Ejemplo**:
     ```python
     import os

     os.chdir("/ruta/a/otro/directorio")
     print("Directorio actual:", os.getcwd())
     ```

**2. Ejercicio de Navegación**
   - **Objetivo**: Crear un script que cambie al directorio de Documentos de tu sistema.
   - **Paso 1**: Escribe el siguiente código en un archivo llamado `cambiar_directorio.py`:
     ```python
     import os

     # Cambia al directorio de Documentos (reemplaza la ruta con la correcta para tu sistema)
     os.chdir("/ruta/a/Documentos")
     print("Directorio actual:", os.getcwd())
     ```
   - **Paso 2**: Ejecuta el archivo y verifica que el directorio actual ha cambiado.

---

### 3.6 Desafío de Programación: Organizador de Archivos

**Objetivo**: Combinar todo lo aprendido para crear un organizador de archivos que mueva los archivos en función de su tipo a carpetas específicas (por ejemplo, `Imágenes`, `Documentos`).

**Instrucciones**
1. Crea un archivo llamado `organizador_archivos.py`.
2. Escribe el siguiente código:
   ```python
   import os
   import shutil

   # Directorio actual y carpetas para organizar
   directorio_actual = "."
   carpetas = {
       "Imágenes": [".jpg", ".png"],
       "Documentos": [".pdf", ".docx", ".txt"],
       "Música": [".mp3", ".wav"]
   }

   # Crear carpetas si no existen
   for carpeta in carpetas:
       if not os.path.exists(carpeta):
           os.mkdir(carpeta)

   # Mover archivos a carpetas correspondientes
   for archivo in os.listdir(directorio_actual):
       nombre, extension = os.path.splitext(archivo)
       for carpeta, extensiones in carpetas.items():
           if extension in extensiones:
               shutil.move(archivo, carpeta)
               print(f"Movido {archivo} a {carpeta}")
   ```
3. Ejecuta el script y verifica que los archivos en el directorio actual se mueven a las carpetas correctas según su extensión.
