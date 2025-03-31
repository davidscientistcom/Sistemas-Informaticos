### Capítulo 1: Introducción a Python en la Administración de Sistemas Operativos

#### Objetivo del Capítulo
Este capítulo te ayudará a:
1. Conocer qué es Python y por qué es importante en la administración de sistemas operativos.
2. Aprender a verificar si Python está instalado en tu sistema e instalarlo si no lo está.
3. Escribir tu primer programa en Python para comenzar a familiarizarte con el lenguaje.

---

### 1.1 ¿Qué es Python y por qué es útil en la administración de sistemas?

**1. ¿Qué es Python?**
   - Python es un lenguaje de programación fácil de aprender y usar, especialmente útil para tareas de administración de sistemas. Su sencillez y versatilidad permiten a los administradores automatizar tareas, realizar configuraciones y monitorizar sistemas de manera eficiente.
   
**2. ¿Por qué Python es útil en sistemas operativos?**
   - Imagina que quieres verificar todos los archivos de una carpeta y ver sus nombres: puedes hacerlo manualmente, o puedes crear un script en Python que haga el trabajo automáticamente. Python puede ayudarte a automatizar tareas repetitivas y a realizar funciones avanzadas, como verificar el uso de espacio en disco, gestionar usuarios o supervisar el estado de la red.
   
**3. Ejercicio de Introducción**
   - Vamos a probar un pequeño programa en Python para listar los archivos que tienes en tu directorio actual.
   - **Paso 1**: Abre tu terminal.
   - **Paso 2**: Escribe el siguiente código y observa lo que hace.
      ```python
      import os

      # Este script listará los archivos en el directorio actual
      print("Archivos en el directorio actual:")
      for archivo in os.listdir("."):
          print(archivo)
      ```
   - **Paso 3**: Ejecuta el script en tu terminal y revisa el resultado. ¿Ves todos los archivos listados? Esto es un ejemplo de cómo Python puede ayudarte a gestionar tu sistema de archivos.

---

### 1.2 Verificar e Instalar Python en tu Sistema Operativo

**Objetivo de esta sección**: Asegurarte de que Python está instalado en tu equipo. Si no lo tienes, te enseñaremos cómo instalarlo.

#### 1. ¿Cómo verificar si tienes Python instalado?
   - Abre tu terminal (en Windows, busca “cmd” o “PowerShell”; en macOS y Linux, abre “Terminal”).
   - Escribe el siguiente comando y presiona Enter:
     ```bash
     python3 --version
     ```
   - **Resultado esperado**:
      - Si Python está instalado, deberías ver algo como: `Python 3.x.x`.
      - Si recibes un error diciendo que `python3` no se encuentra, sigue las instrucciones del siguiente paso para instalarlo.

#### 2. Instalación de Python
   - **Windows**:
      - Ve a [python.org](https://www.python.org/downloads/) y descarga el instalador de Python.
      - Ejecuta el instalador y asegúrate de marcar la opción “Add Python to PATH” antes de instalar.
      - Reinicia la terminal y vuelve a ejecutar `python3 --version` para verificar que Python se haya instalado correctamente.
   
   - **macOS**:
      - Python 3 viene preinstalado en las versiones modernas de macOS, pero si quieres actualizarlo, puedes hacerlo desde el sitio web oficial.
      - Alternativamente, puedes instalar `Homebrew` (un gestor de paquetes) y luego escribir:
        ```bash
        brew install python
        ```
   
   - **Linux (Ubuntu/Debian)**:
      - Ejecuta el siguiente comando en la terminal:
        ```bash
        sudo apt update
        sudo apt install python3
        ```
      - Esto instalará la versión más reciente de Python 3 disponible para tu distribución de Linux.

#### 3. Ejercicio de Verificación
   - Una vez instalado Python, vuelve a ejecutar el comando `python3 --version`.
   - **Desafío**: Si logras instalar Python correctamente, intenta ejecutar el siguiente código en la terminal:
     ```python
     print("¡Python está instalado y funcionando!")
     ```

---

### 1.3 Primer Script en Python: “Hola, mundo”

**Objetivo**: Crear y ejecutar tu primer script en Python, lo que te permitirá familiarizarte con la estructura básica de un programa en este lenguaje.

#### 1. Crear el script
   - Abre cualquier editor de texto (por ejemplo, `Bloc de notas` en Windows, `TextEdit` en macOS o `nano` en Linux).
   - Escribe el siguiente código:
     ```python
     print("Hola, mundo")
     ```
   - Guarda el archivo con el nombre `hola_mundo.py`.

#### 2. Ejecutar el script
   - Abre la terminal y navega a la carpeta donde guardaste el archivo `hola_mundo.py`.
   - Escribe el siguiente comando para ejecutar el script:
     ```bash
     python3 hola_mundo.py
     ```
   - **Resultado esperado**: Deberías ver la salida `Hola, mundo` en la terminal. ¡Felicidades, acabas de ejecutar tu primer programa en Python!

#### 3. Ejercicio de Ampliación
   - Modifica el script `hola_mundo.py` para que muestre una lista de tres cosas que te gustaría aprender en este curso. Ejemplo:
     ```python
     print("1. Automatización de tareas")
     print("2. Monitorización del sistema")
     print("3. Administración de usuarios")
     ```
   - Guarda y ejecuta el script para ver los resultados.

### 1.4 ¿Qué son las librerías en Python y por qué son importantes?
**Objetivo**: Entender el concepto de librerías en Python, cómo utilizarlas y su importancia en la administración de sistemas operativos.

#### 1. ¿Qué es una librería?
Una librería en Python es un conjunto de módulos (archivos de código Python) que contienen funciones, clases y variables predefinidas que puedes usar en tus programas. Las librerías te ahorran tiempo y esfuerzo al proporcionar herramientas ya creadas para tareas comunes.

Por ejemplo:
- **`os`**: Permite interactuar con el sistema operativo (como listar archivos, crear carpetas, etc.).
- **`sys`**: Ofrece acceso a funcionalidades específicas del intérprete de Python.
- **`subprocess`**: Permite ejecutar comandos del sistema operativo desde Python.

#### 2. ¿Por qué son importantes en la administración de sistemas?
Las librerías permiten:
- **Automatizar tareas repetitivas**: Por ejemplo, con `os` puedes automatizar la gestión de archivos y directorios.
- **Interactuar con el sistema operativo**: Usar librerías como `subprocess` para ejecutar comandos.
- **Monitorizar recursos del sistema**: Con librerías como `psutil` puedes supervisar el uso de CPU, memoria, disco, etc.

#### 3. Ejemplo práctico
Vamos a usar la librería `os` para crear un directorio, listar su contenido y luego eliminarlo.

```python
import os

# Crear un directorio
os.mkdir("directorio_prueba")
print("Directorio 'directorio_prueba' creado.")

# Listar contenido del directorio actual
print("Contenido actual del directorio:")
print(os.listdir("."))

# Eliminar el directorio
os.rmdir("directorio_prueba")
print("Directorio 'directorio_prueba' eliminado.")
```

Ejecuta este código y observa cómo Python interactúa directamente con el sistema operativo.

---

### 1.5 ¿Qué son los entornos virtuales en Python y para qué sirven?
**Objetivo**: Aprender qué son los entornos virtuales, cómo crearlos y por qué son útiles en la administración de sistemas.

#### 1. ¿Qué es un entorno virtual?
Un entorno virtual es un espacio aislado donde puedes instalar librerías y paquetes de Python sin afectar la instalación global en tu sistema. Esto es especialmente útil para:
- Evitar conflictos entre versiones de paquetes.
- Crear entornos específicos para cada proyecto.
- Facilitar la portabilidad de scripts entre sistemas.

#### 2. ¿Cómo crear un entorno virtual?
1. Abre tu terminal.
2. Navega al directorio donde deseas crear el entorno.
3. Escribe el siguiente comando para crear un entorno virtual:
   ```bash
   python3 -m venv mi_entorno
   ```
   Esto creará una carpeta llamada `mi_entorno` con el entorno virtual.

4. Para activar el entorno virtual:
   - En **Linux/macOS**:
     ```bash
     source mi_entorno/bin/activate
     ```
   - En **Windows**:
     ```bash
     .\mi_entorno\Scripts\activate
     ```

5. Una vez activado, verás el nombre del entorno en el prompt de tu terminal, indicando que estás trabajando dentro del entorno virtual.

6. Para instalar librerías dentro del entorno, usa `pip`:
   ```bash
   pip install nombre_libreria
   ```

7. Para desactivar el entorno, usa:
   ```bash
   deactivate
   ```

#### 3. Ejercicio práctico
Crea un entorno virtual y dentro de él instala la librería `psutil`. Luego, escribe un script que muestre el porcentaje de uso de CPU en tu sistema.

Código de ejemplo:
```python
import psutil

# Mostrar porcentaje de uso de CPU
print("Uso de CPU: ", psutil.cpu_percent(interval=1), "%")
```

---

### 1.6 Evolución de Python y el cambio de Python 2 a Python 3
**Objetivo**: Comprender los cambios principales en Python 3 y por qué fue necesario pasar de Python 2 a Python 3.

#### 1. Breve historia de Python
- Python fue creado por **Guido van Rossum** en 1991.
- Python 2 fue lanzado en 2000 y se convirtió en una versión muy popular debido a su simplicidad y versatilidad.
- Python 3 fue lanzado en 2008 con mejoras significativas, pero no era compatible con Python 2, lo que provocó una transición gradual y compleja.

#### 2. ¿Por qué fue necesario Python 3?
Python 2 tenía varias limitaciones y problemas de diseño que no podían solucionarse sin romper la compatibilidad con versiones anteriores. Python 3 introdujo mejoras como:
- **Soporte completo para Unicode**: En Python 3, las cadenas son Unicode por defecto, lo que facilita trabajar con diferentes idiomas y conjuntos de caracteres.
- **Mejoras en la división de números**: En Python 2, dividir dos enteros podía producir un número entero. En Python 3, siempre produce un número flotante.
   ```python
   # Python 2
   print(5 / 2)  # Resultado: 2
   # Python 3
   print(5 / 2)  # Resultado: 2.5
   ```
- **Funciones `print` y otras mejoras sintácticas**: En Python 3, `print` es una función, lo que permite mayor consistencia y flexibilidad.
   ```python
   # Python 2
   print "Hola, mundo"
   # Python 3
   print("Hola, mundo")
   ```
- **Eliminación de características obsoletas**: Python 3 eliminó características redundantes o poco usadas, haciendo el lenguaje más limpio y eficiente.

#### 3. Fin de soporte para Python 2
El soporte oficial para Python 2 finalizó el **1 de enero de 2020**, lo que significa que ya no recibe actualizaciones ni parches de seguridad.

#### 4. Ejercicio práctico: Compatibilidad entre Python 2 y Python 3
Prueba este código en Python 3 para observar cómo cambió el uso de `print`:
```python
# Ejemplo de uso de print en Python 3
print("Hola, mundo")
```
