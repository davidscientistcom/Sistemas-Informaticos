### Ejercicios de Condicionales y Bucles con Operadores Lógicos

A continuación, se presentan ejercicios diseñados para comprender el uso de los operadores lógicos `and`, `or`, y `not` en combinación con condicionales y bucles.



### **Ejercicio 1: Verificar si un número está dentro de un rango**
**Objetivo**: Determinar si un número ingresado por el usuario está dentro de un rango específico usando operadores `and`.

#### Código:
```python
numero = int(input("Introduce un número: "))

# Verificar si el número está entre 10 y 20
if numero >= 10 and numero <= 20:
    print("El número está en el rango de 10 a 20.")
else:
    print("El número está fuera del rango de 10 a 20.")
```

---

### **Ejercicio 2: Verificar múltiplos de dos números**
**Objetivo**: Determinar si un número es múltiplo de 3 o 5 usando el operador `or`.

#### Código:
```python
numero = int(input("Introduce un número: "))

if numero % 3 == 0 or numero % 5 == 0:
    print("El número es múltiplo de 3 o 5.")
else:
    print("El número no es múltiplo de 3 ni de 5.")
```

---

### **Ejercicio 3: Números pares y mayores que un valor**
**Objetivo**: Mostrar números del 1 al 50 que sean pares y mayores que 30 usando un bucle y el operador `and`.

#### Código:
```python
print("Números pares mayores que 30:")
for numero in range(1, 51):
    if numero % 2 == 0 and numero > 30:
        print(numero)
```

---

### **Ejercicio 4: Filtrar valores con operadores lógicos**
**Objetivo**: Filtrar números del 1 al 100 que sean divisibles por 3 o 5 pero no por ambos.

#### Código:
```python
print("Números divisibles por 3 o 5 pero no por ambos:")
for numero in range(1, 101):
    if (numero % 3 == 0 or numero % 5 == 0) and not (numero % 3 == 0 and numero % 5 == 0):
        print(numero)
```

---

### **Ejercicio 5: Validar contraseñas seguras**
**Objetivo**: Pedir al usuario que ingrese una contraseña y verificar que cumpla con condiciones específicas usando operadores lógicos.

#### Código:
```python
contraseña = input("Introduce una contraseña: ")

# Condiciones de una contraseña segura
mayuscula = any(letra.isupper() for letra in contraseña)
minuscula = any(letra.islower() for letra in contraseña)
numero = any(letra.isdigit() for letra in contraseña)

if mayuscula and minuscula and numero and len(contraseña) >= 8:
    print("Contraseña válida.")
else:
    print("Contraseña inválida. Debe tener al menos una mayúscula, una minúscula, un número y 8 caracteres.")
```

---

### **Ejercicio 6: Números primos en un rango**
**Objetivo**: Usar un bucle y operadores lógicos para encontrar números primos en un rango.

#### Código:
```python
print("Números primos entre 1 y 50:")

for numero in range(1, 51):
    if numero > 1:  # Los números primos son mayores que 1
        es_primo = True
        for divisor in range(2, int(numero ** 0.5) + 1):
            if numero % divisor == 0:
                es_primo = False
                break
        if es_primo:
            print(numero)
```

---

### **Ejercicio 7: Encuentra números especiales**
**Objetivo**: Encontrar números entre 1 y 100 que sean divisibles por 4 y no por 6, o que sean divisibles por ambos.

#### Código:
```python
print("Números especiales entre 1 y 100:")

for numero in range(1, 101):
    if (numero % 4 == 0 and numero % 6 != 0) or (numero % 4 == 0 and numero % 6 == 0):
        print(numero)
```

---

### **Ejercicio 8: Tabla de números con condiciones**
**Objetivo**: Mostrar una tabla de números del 1 al 20 y marcar cuáles cumplen con dos condiciones: ser par y mayor que 10.

#### Código:
```python
print("Tabla de números del 1 al 20:")
print("Número\tPar y > 10")
print("----------------------")
for numero in range(1, 21):
    cumple = "Sí" if numero % 2 == 0 and numero > 10 else "No"
    print(f"{numero}\t{cumple}")
```

---

### **Ejercicio 9: Juego de adivinanza con lógica**
**Objetivo**: Crear un juego donde el usuario adivine un número secreto cumpliendo varias condiciones.

#### Código:
```python
numero_secreto = 42
intentos = 0
max_intentos = 5

while intentos < max_intentos:
    numero = int(input("Adivina el número secreto (entre 1 y 100): "))
    intentos += 1

    if numero == numero_secreto:
        print("¡Correcto! Adivinaste el número.")
        break
    elif numero < numero_secreto and numero % 2 == 0:
        print("Demasiado bajo, pero estás cerca.")
    elif numero > numero_secreto or numero % 2 != 0:
        print("Demasiado alto o el número no es par.")
else:
    print("¡Has agotado todos los intentos!")
```

---

### **Ejercicio 10: Crear un patrón condicional**
**Objetivo**: Dibujar un patrón de asteriscos en un cuadrado solo si la posición cumple ciertas condiciones.

#### Código:
```python
tamaño = int(input("Introduce el tamaño del cuadrado: "))

for fila in range(tamaño):
    for columna in range(tamaño):
        if fila == columna or fila + columna == tamaño - 1:
            print("*", end="")
        else:
            print(" ", end="")
    print()
```

#### Ejemplo de Ejecución:
Si introduces `tamaño = 5`, el resultado será:
```
*   *
 * * 
  *  
 * * 
*   *
```
