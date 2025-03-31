# Guía Detallada de Argparse en Python

El módulo `argparse` de Python es una herramienta poderosa para construir interfaces de línea de comandos. Permite definir y manejar argumentos y opciones que un usuario puede pasar a un programa desde la consola. Esto es especialmente útil cuando se desarrollan aplicaciones de consola.

En esta guía aprenderás cómo usar `argparse` para crear programas que acepten parámetros y opciones, con ejemplos detallados y explicaciones claras.



## Introducción a Argparse

El módulo `argparse` permite:

1. Definir argumentos y opciones para el programa.
2. Generar mensajes de ayuda automáticamente.
3. Validar tipos de datos y gestionar errores.

### Ejemplo Básico
Supongamos que queremos crear un programa que calcule el área de un rectángulo. El programa debe aceptar el ancho y el alto como argumentos.

```python
import argparse

# Crear el analizador de argumentos
parser = argparse.ArgumentParser(description="Calcula el área de un rectángulo.")

# Definir los argumentos
parser.add_argument("--ancho", type=float, required=True, help="Ancho del rectángulo")
parser.add_argument("--alto", type=float, required=True, help="Alto del rectángulo")

# Parsear los argumentos
args = parser.parse_args()

# Calcular y mostrar el área
area = args.ancho * args.alto
print(f"El área del rectángulo es: {area}")
```

Ejecuta este programa desde la terminal:

```bash
python programa.py --ancho 5 --alto 10
```

Resultado:
```
El área del rectángulo es: 50.0
```



## Partes Principales de Argparse

### 1. Crear el analizador

```python
parser = argparse.ArgumentParser(description="Descripción del programa")
```

El argumento `description` permite agregar un mensaje explicativo sobre la funcionalidad del programa.

### 2. Agregar argumentos

Los argumentos se definen usando el método `add_argument()`.

```python
parser.add_argument("--nombre", type=str, help="Nombre del usuario")
```

#### Principales parámetros de `add_argument()`:
- `name or flags`: El nombre del argumento (e.g., `--nombre`).
- `type`: El tipo de dato esperado (e.g., `int`, `float`, `str`).
- `help`: Una descripción del argumento para los mensajes de ayuda.
- `required`: Si el argumento es obligatorio (`True`) o no (`False`).
- `default`: Un valor por defecto si el argumento no se proporciona.

### 3. Parsear argumentos

El método `parse_args()` lee los argumentos proporcionados desde la línea de comandos.

```python
args = parser.parse_args()
print(args.nombre)  # Accede al valor del argumento
```



## Tipos de Argumentos

### 1. Argumentos Posicionales

Son argumentos que no llevan prefijos como `--` y se identifican por su posición.

```python
parser.add_argument("ancho", type=float, help="Ancho del rectángulo")
parser.add_argument("alto", type=float, help="Alto del rectángulo")
```

Ejemplo:
```bash
python programa.py 5 10
```

### 2. Argumentos Opcionales

Son argumentos precedidos por `--` o `-` y no son obligatorios a menos que se especifique lo contrario.

```python
parser.add_argument("--verbose", action="store_true", help="Activa el modo verboso")
```

Si se incluye `--verbose` en la línea de comandos, su valor será `True`.

```bash
python programa.py --verbose
```

### 3. Valores por Defecto

Puedes asignar un valor por defecto si el usuario no proporciona el argumento.

```python
parser.add_argument("--factor", type=float, default=1.0, help="Factor de ajuste")
```



## Ejemplo Avanzado: Calculadora

Vamos a crear una calculadora que acepte tres argumentos: dos números y una operación.

```python
import argparse

parser = argparse.ArgumentParser(description="Calculadora básica")

parser.add_argument("numero1", type=float, help="Primer número")
parser.add_argument("numero2", type=float, help="Segundo número")
parser.add_argument("--operacion", type=str, choices=["suma", "resta", "multiplica", "divide"], default="suma", help="Operación a realizar")

args = parser.parse_args()

if args.operacion == "suma":
    resultado = args.numero1 + args.numero2
elif args.operacion == "resta":
    resultado = args.numero1 - args.numero2
elif args.operacion == "multiplica":
    resultado = args.numero1 * args.numero2
elif args.operacion == "divide":
    if args.numero2 != 0:
        resultado = args.numero1 / args.numero2
    else:
        resultado = "Error: división por cero"

print(f"El resultado de la {args.operacion} es: {resultado}")
```

Ejemplo de ejecución:
```bash
python calculadora.py 10 5 --operacion resta
```

Salida:
```
El resultado de la resta es: 5.0
```



## Argumentos Mutuamente Excluyentes

Puedes definir grupos de argumentos donde solo se permita uno a la vez usando `add_mutually_exclusive_group()`.

```python
parser = argparse.ArgumentParser(description="Opciones excluyentes")
group = parser.add_mutually_exclusive_group()
group.add_argument("--modo1", action="store_true", help="Activa el modo 1")
group.add_argument("--modo2", action="store_true", help="Activa el modo 2")

args = parser.parse_args()

if args.modo1:
    print("Modo 1 activado")
elif args.modo2:
    print("Modo 2 activado")
```



## Generar Mensajes de Ayuda

`argparse` genera automáticamente un mensaje de ayuda basado en los argumentos definidos.

```bash
python programa.py --help
```

Salida:
```
usage: programa.py [-h] --ancho ANCHO --alto ALTO

Calcula el área de un rectángulo.

options:
  -h, --help     show this help message and exit
  --ancho ANCHO  Ancho del rectángulo
  --alto ALTO    Alto del rectángulo
```
