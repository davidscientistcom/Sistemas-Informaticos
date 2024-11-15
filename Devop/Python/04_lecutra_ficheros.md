### Capítulo 4: Lectura y Escritura de Archivos

#### Objetivo del Capítulo
Aprenderás cómo leer datos de archivos y escribir en ellos utilizando Python. Estas habilidades te permitirán gestionar archivos de registro, analizar datos y crear archivos de configuración.

---

### 4.1 Introducción a la Lectura y Escritura de Archivos

**1. ¿Por qué es importante leer y escribir archivos?**
   - En la administración de sistemas, muchas tareas involucran la manipulación de archivos de texto: crear registros, modificar configuraciones y analizar datos.
   - Python proporciona métodos sencillos para leer y escribir archivos, lo cual facilita su uso en tareas de mantenimiento y automatización.

---

### 4.2 Abrir y Cerrar Archivos

**1. Abrir un Archivo en Python**
   - Para trabajar con un archivo en Python, primero debes abrirlo usando la función `open()`, que toma dos argumentos: el nombre del archivo y el modo.
   - **Modos de apertura**:
      - `"r"`: Modo de lectura (read).
      - `"w"`: Modo de escritura (write). Crea un archivo nuevo si no existe o lo sobreescribe si ya existe.
      - `"a"`: Modo de agregar (append). Añade información al final del archivo sin sobreescribirlo.
      - `"r+"`: Modo de lectura y escritura.

**2. Ejemplo: Abrir y Cerrar un Archivo**
   ```python
   archivo = open("mi_archivo.txt", "w")  # Abre el archivo en modo escritura
   archivo.close()  # Cierra el archivo
   ```

**3. Ejercicio**
   - **Objetivo**: Crea un archivo `abrir_cerrar.py` que abra un archivo llamado `ejemplo.txt` en modo escritura, luego lo cierre, y verifica que el archivo ha sido creado en el directorio actual.

---

### 4.3 Escribir en Archivos

Para escribir en un archivo, utilizamos el método `write()` después de abrirlo en modo `"w"` o `"a"`.

**Ejemplo de Escritura**
   ```python
   archivo = open("mi_archivo.txt", "w")
   archivo.write("¡Hola, mundo! Este es mi primer archivo escrito con Python.\n")
   archivo.close()
   ```
   
**Ejercicio: Crear un Archivo de Registro**
   - **Objetivo**: Escribe un script que cree un archivo llamado `registro.txt` y añada una línea de texto con un mensaje personalizado.
   - **Paso 1**: Escribe el siguiente código en un archivo `escribir_archivo.py`:
     ```python
     archivo = open("registro.txt", "w")
     archivo.write("Registro de eventos del sistema\n")
     archivo.write("Evento: Inicio de sesión - Hora: 10:00 AM\n")
     archivo.close()
     ```
   - **Paso 2**: Ejecuta el script y abre `registro.txt` para verificar el contenido.

---

### 4.4 Leer Archivos

Para leer un archivo, lo abrimos en modo `"r"` y utilizamos métodos como `read()`, `readline()` o `readlines()`.

**1. Método `read()`**: Lee todo el contenido del archivo.
   ```python
   archivo = open("registro.txt", "r")
   contenido = archivo.read()
   print(contenido)
   archivo.close()
   ```

**2. Método `readline()`**: Lee una línea a la vez, ideal para archivos largos.
   ```python
   archivo = open("registro.txt", "r")
   linea = archivo.readline()
   while linea:
       print(linea, end="")  # Evita el doble salto de línea
       linea = archivo.readline()
   archivo.close()
   ```

**3. Ejercicio: Leer Archivo Línea por Línea**
   - **Objetivo**: Crea un archivo `leer_archivo.py` que lea cada línea de `registro.txt` y la muestre en la pantalla.
   - **Paso 1**: Escribe el código anterior en el archivo `leer_archivo.py`.
   - **Paso 2**: Ejecuta el archivo y observa cómo se imprimen las líneas del archivo.

---

### 4.5 Agregar Información a un Archivo

Para añadir contenido sin sobrescribir el archivo existente, usa el modo `"a"` (append).

**Ejemplo de Agregar Información**
   ```python
   archivo = open("registro.txt", "a")
   archivo.write("Evento: Cierre de sesión - Hora: 11:00 AM\n")
   archivo.close()
   ```

**Ejercicio: Actualizar el Archivo de Registro**
   - **Objetivo**: Escribe un script que agregue un nuevo evento al archivo `registro.txt`.
   - **Paso 1**: Escribe el siguiente código en `actualizar_registro.py`:
     ```python
     archivo = open("registro.txt", "a")
     archivo.write("Evento: Error en el sistema - Hora: 12:00 PM\n")
     archivo.close()
     ```
   - **Paso 2**: Ejecuta el script y revisa `registro.txt` para ver la nueva línea añadida.

---

### 4.6 Ejercicio Completo: Generador de Archivos de Log

**Objetivo**: Crear un archivo de registro automático que añada entradas de eventos en función de las acciones que realices en el sistema.

1. **Paso 1**: Crea un archivo llamado `generador_logs.py`.
2. **Paso 2**: Escribe el siguiente código:
   ```python
   import datetime

   def registrar_evento(evento):
       with open("logs.txt", "a") as archivo:
           hora_actual = datetime.datetime.now().strftime("%Y-%m-%d %H:%M:%S")
           archivo.write(f"{hora_actual} - {evento}\n")

   registrar_evento("Inicio del sistema")
   registrar_evento("Usuario autenticado")
   registrar_evento("Error: Archivo no encontrado")
   ```
3. **Paso 3**: Ejecuta el script y abre `logs.txt` para ver el registro generado.

**Explicación del código**:
   - Usamos la función `registrar_evento()` para crear una entrada en el archivo `logs.txt` con la fecha y hora actual.
   - Cada vez que ejecutas el script, nuevos eventos se añadirán al archivo de logs.

---

### 4.7 Modo `with` para Manejar Archivos

Python ofrece una forma más segura de abrir archivos usando la instrucción `with`, que asegura que el archivo se cierre automáticamente al finalizar su uso.

**Ejemplo con `with`**
   ```python
   with open("mi_archivo.txt", "r") as archivo:
       contenido = archivo.read()
       print(contenido)
   ```
   - Aquí no es necesario cerrar el archivo explícitamente; `with` lo hace automáticamente al finalizar el bloque.

**Ejercicio**
   - **Objetivo**: Modifica el código en `leer_archivo.py` para usar el modo `with` al abrir `registro.txt`.
   - **Paso 1**: Cambia el código en `leer_archivo.py` para que quede así:
     ```python
     with open("registro.txt", "r") as archivo:
         for linea in archivo:
             print(linea, end="")
     ```
   - **Paso 2**: Ejecuta el script y verifica que funcione como antes, pero sin la necesidad de cerrar el archivo manualmente.
