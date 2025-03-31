### **Creación de funciones en Python**

En Python, las funciones se utilizan para agrupar un conjunto de instrucciones que se pueden ejecutar varias veces. Esto nos ayuda a evitar repetir código y hace que el programa sea más organizado y fácil de entender.

#### **Cómo crear una función**
1. Se utiliza la palabra clave `def` para definir una función.
2. El nombre de la función debe ser descriptivo y seguir las reglas de nombres de variables (no usar espacios, empezar con una letra o guion bajo, etc.).
3. Entre paréntesis, podemos incluir **parámetros** que son valores que la función necesita para trabajar.
4. El código de la función debe estar indentado (generalmente con 4 espacios).
5. Para que una función devuelva un valor, se utiliza la palabra clave `return`.

#### **Estructura básica**
```python
def nombre_de_la_funcion(parametro1, parametro2):
    # Código que hace algo
    resultado = parametro1 + parametro2
    return resultado
```

### **Parámetros de una función**
- **Parámetros**: Son variables que se declaran dentro de los paréntesis al definir una función. Son datos que la función necesita para realizar su tarea.
- **Argumentos**: Son los valores reales que se pasan a los parámetros cuando se llama a la función.


### **Ejemplos**

#### **Ejemplo 1: Función sin parámetros**
```python
def saludar():
    print("¡Hola! Bienvenido al curso de Python.")

# Llamamos a la función
saludar()
```

#### **Ejemplo 2: Función con un parámetro**
```python
def saludar(nombre):
    print(f"¡Hola, {nombre}! Espero que estés disfrutando el curso.")

# Llamamos a la función pasando un argumento
saludar("María")
saludar("Juan")
```

#### **Ejemplo 3: Función con dos parámetros**
```python
def sumar(a, b):
    resultado = a + b
    return resultado

# Llamamos a la función y mostramos el resultado
suma = sumar(4, 5)
print(f"La suma de 4 y 5 es: {suma}")
```

#### **Ejemplo 4: Función con parámetros y operaciones**
```python
def calcular_area_rectangulo(base, altura):
    area = base * altura
    return area

# Llamamos a la función y usamos el resultado
area = calcular_area_rectangulo(10, 5)
print(f"El área del rectángulo es: {area} unidades cuadradas.")
```

#### **Ejemplo 5: Función que verifica un número**
```python
def es_par(numero):
    if numero % 2 == 0:
        return True
    else:
        return False

# Llamamos a la función y verificamos
numero = 8
if es_par(numero):
    print(f"El número {numero} es par.")
else:
    print(f"El número {numero} es impar.")
```



### **Ejercicios para practicar**
1. Crea una función llamada `multiplicar` que reciba dos números y devuelva su producto.
2. Escribe una función que reciba un nombre y lo salude de forma personalizada, por ejemplo: "¡Hola, Marta! Espero que tengas un buen día".
3. Haz una función que calcule el área de un círculo dado su radio (usa la fórmula `π * radio^2` y asume que π = 3.1416).
4. Escribe una función que reciba un número y diga si es positivo, negativo o cero.
5. Define una función que reciba tres números y devuelva el mayor de ellos.


Aquí tienes un ejemplo sencillo que muestra primero cómo resolver un problema sin funciones y luego cómo refactorizar el código utilizando funciones, incluyendo una función que llama a otras funciones.



## Refactorización

Usamos funciones para **"encapsular"** funcionalidad y poder reusar.

### **Ejemplo: Calcular el área y el perímetro de un rectángulo**

#### **Versión sin funciones**
```python
# Pedir datos al usuario
base = float(input("Introduce la base del rectángulo: "))
altura = float(input("Introduce la altura del rectángulo: "))

# Calcular el área
area = base * altura

# Calcular el perímetro
perimetro = 2 * (base + altura)

# Mostrar los resultados
print(f"El área del rectángulo es: {area}")
print(f"El perímetro del rectángulo es: {perimetro}")
```

Este código funciona, pero si más adelante queremos reutilizar partes del cálculo, como el área o el perímetro, tendremos que repetir el mismo código varias veces. Ahora, vamos a refactorizarlo utilizando funciones.



#### **Versión refactorizada con funciones**
```python
# Definir una función para calcular el área
def calcular_area(base, altura):
    return base * altura

# Definir una función para calcular el perímetro
def calcular_perimetro(base, altura):
    return 2 * (base + altura)

# Función principal que llama a las otras funciones
def calcular_rectangulo():
    # Pedir datos al usuario
    base = float(input("Introduce la base del rectángulo: "))
    altura = float(input("Introduce la altura del rectángulo: "))

    # Usar las funciones para calcular el área y el perímetro
    area = calcular_area(base, altura)
    perimetro = calcular_perimetro(base, altura)

    # Mostrar los resultados
    print(f"El área del rectángulo es: {area}")
    print(f"El perímetro del rectángulo es: {perimetro}")

# Llamar a la función principal
calcular_rectangulo()
```



### **Explicación del refactorizado**
1. **Funciones individuales**:
   - `calcular_area(base, altura)`: Se encarga únicamente de calcular el área, lo que la hace reutilizable si necesitamos el área en otro contexto.
   - `calcular_perimetro(base, altura)`: Calcula el perímetro y también es reutilizable.
   
2. **Función principal (`calcular_rectangulo`)**:
   - Gestiona todo el flujo del programa: solicita los datos al usuario, llama a las funciones para los cálculos y muestra los resultados.

3. **Beneficio del uso de funciones**:
   - **Reutilización**: Si necesitas calcular el área o el perímetro en otro lugar del programa, puedes usar las funciones directamente sin repetir el código.
   - **Modularidad**: El código está dividido en partes más pequeñas y manejables.
   - **Legibilidad**: El flujo del programa principal es más claro, ya que las funciones tienen nombres descriptivos.


**EJERCICIO* Lo que hemos hecho arriba esta bien, pero estamos metiendo la responsabilidad de leer los datos dentro de la función **calcular_rectangulo**, eso esa mal, ya que **OBLIGA** a que nosotros leamos de teclado, pero y si nos vienen las peticiones de un servicio web?, ese código no funcionaria.

**NUNCA DEBEMOS DE HACER CÓDIGO QUE VINCULE LA ENTRADA DEL USUARIO DE ESA FORMA**

Por tanto el ejercicio. Refactorizar para hacerlo de forma correcta.