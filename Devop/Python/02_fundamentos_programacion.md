### Capítulo 2: Fundamentos de Programación en Python

#### Objetivo del Capítulo
En este capítulo, aprenderás los fundamentos de la programación en Python. Estos conocimientos te permitirán crear scripts básicos y sentar la base para trabajar con tareas de administración en el sistema operativo.

---

### 2.1 Variables y Tipos de Datos en Python

**1. ¿Qué son las variables?**
   - Una **variable** es un nombre que almacena un valor. Las variables nos permiten guardar información en la memoria del ordenador para usarla en diferentes partes del programa.
   - **Ejemplo**: Imagina que necesitas almacenar tu nombre en el programa; podrías crear una variable llamada `nombre` y asignarle tu nombre como valor.
   
**2. Creación de una variable en Python**
   - Para crear una variable en Python, simplemente eliges un nombre y le asignas un valor usando el signo `=`.
     ```python
     nombre = "Juan"
     edad = 25
     ```
   
**3. Ejercicio: Crear tus propias variables**
   - **Paso 1**: Abre tu editor de texto y escribe el siguiente código:
     ```python
     nombre = "Ana"
     edad = 30
     sistema_operativo = "Ubuntu"
     
     print("Nombre:", nombre)
     print("Edad:", edad)
     print("Sistema Operativo:", sistema_operativo)
     ```
   - **Paso 2**: Guarda el archivo como `variables.py` y ejecútalo en la terminal usando:
     ```bash
     python3 variables.py
     ```
   - **Resultado esperado**: El programa debería mostrar el nombre, la edad y el sistema operativo que introdujiste.

**4. Tipos de datos básicos en Python**
   - **Entero (int)**: Números enteros, como `10`, `-3`, `45`.
   - **Flotante (float)**: Números decimales, como `3.14`, `-0.5`, `2.71`.
   - **Cadena (str)**: Texto, como `"Hola"` o `"Python"`.
   - **Booleano (bool)**: Valores de verdadero o falso (`True` o `False`).

---

### 2.2 Operadores Básicos

**1. Operadores Aritméticos**
   - Python permite realizar operaciones matemáticas básicas.
     ```python
     a = 10
     b = 3
     
     suma = a + b
     resta = a - b
     multiplicacion = a * b
     division = a / b
     
     print("Suma:", suma)
     print("Resta:", resta)
     print("Multiplicación:", multiplicacion)
     print("División:", division)
     ```

**2. Ejercicio: Calculadora Simple**
   - **Objetivo**: Crear un script que haga sumas y multiplicaciones usando variables.
   - **Paso 1**: Escribe el siguiente código:
     ```python
     numero1 = 15
     numero2 = 7
     
     resultado_suma = numero1 + numero2
     resultado_multiplicacion = numero1 * numero2
     
     print("Resultado de la suma:", resultado_suma)
     print("Resultado de la multiplicación:", resultado_multiplicacion)
     ```
   - **Paso 2**: Guarda el archivo como `calculadora.py` y ejecútalo. Observa cómo Python usa operadores para realizar cálculos.

---

### 2.3 Estructuras de Control: Condicionales y Bucles

#### Condicionales `if`
   - Los **condicionales** permiten que el programa ejecute ciertas acciones solo si se cumple una condición específica.
   - **Ejemplo**:
     ```python
     edad = 18
     
     if edad >= 18:
         print("Eres mayor de edad")
     else:
         print("Eres menor de edad")
     ```

**Ejercicio de Condicionales**
   - **Objetivo**: Escribir un script que determine si un número es positivo, negativo o cero.
   - **Paso 1**: Escribe el siguiente código:
     ```python
     numero = int(input("Introduce un número: "))
     
     if numero > 0:
         print("El número es positivo")
     elif numero < 0:
         print("El número es negativo")
     else:
         print("El número es cero")
     ```
   - **Paso 2**: Guarda el archivo como `condicionales.py` y ejecútalo. Introduce distintos números para ver cómo funciona.

#### Bucles `for` y `while`
   - **Bucles `for`**: Se usan cuando conocemos el número de repeticiones.
     ```python
     for i in range(5):
         print("Este es el mensaje número", i)
     ```
   - **Bucles `while`**: Se usan cuando la repetición depende de una condición.
     ```python
     contador = 0
     while contador < 5:
         print("Contador:", contador)
         contador += 1
     ```

**Ejercicio de Bucles**
   - **Objetivo**: Crear un script que imprima los números del 1 al 10 usando un bucle `for`.
   - **Paso 1**: Escribe el siguiente código:
     ```python
     for numero in range(1, 11):
         print(numero)
     ```
   - **Paso 2**: Guarda el archivo como `bucle_for.py` y ejecútalo para ver los resultados.

---

### 2.4 Desafío de Programación: Calculadora Mejorada

**Objetivo**: Combinar todo lo aprendido para crear una calculadora que permita al usuario realizar operaciones aritméticas básicas.

1. **Paso 1**: Crea un nuevo archivo `calculadora_mejorada.py`.
2. **Paso 2**: Escribe el siguiente código:
   ```python
   print("Calculadora Básica")
   print("Operaciones disponibles: suma, resta, multiplicacion, division")

   operacion = input("Introduce la operación que quieres realizar: ")
   numero1 = float(input("Introduce el primer número: "))
   numero2 = float(input("Introduce el segundo número: "))

   if operacion == "suma":
       resultado = numero1 + numero2
   elif operacion == "resta":
       resultado = numero1 - numero2
   elif operacion == "multiplicacion":
       resultado = numero1 * numero2
   elif operacion == "division":
       if numero2 != 0:
           resultado = numero1 / numero2
       else:
           resultado = "Error: No se puede dividir entre cero"
   else:
       resultado = "Operación no válida"

   print("Resultado:", resultado)
   ```
3. **Paso 3**: Ejecuta el programa y prueba diferentes operaciones para ver cómo responde el programa.
