### Ejercicio 1: Triángulo a la Izquierda con Asteriscos

**Objetivo**: Crear un triángulo orientado hacia la izquierda utilizando bucles `for` y condicionales `if`.

#### Código:
```python
# Triángulo orientado hacia la izquierda
altura = int(input("Introduce la altura del triángulo: "))

for i in range(1, altura + 1):  # Desde 1 hasta la altura
    print("*" * i)  # Imprime i asteriscos
```

#### Ejemplo de Ejecución:
Si introduces `altura = 5`, el resultado será:
```
*
**
***
****
*****
```

---

### Ejercicio 2: Triángulo a la Derecha con Asteriscos

**Objetivo**: Crear un triángulo orientado hacia la derecha utilizando espacios y asteriscos.

#### Código:
```python
# Triángulo orientado hacia la derecha
altura = int(input("Introduce la altura del triángulo: "))

for i in range(1, altura + 1):  # Desde 1 hasta la altura
    print(" " * (altura - i) + "*" * i)  # Espacios + Asteriscos
```

#### Ejemplo de Ejecución:
Si introduces `altura = 5`, el resultado será:
```
    *
   **
  ***
 ****
*****
```

---

### Ejercicio 3: Triángulo Invertido a la Izquierda

**Objetivo**: Dibujar un triángulo invertido orientado hacia la izquierda.

#### Código:
```python
# Triángulo invertido orientado hacia la izquierda
altura = int(input("Introduce la altura del triángulo: "))

for i in range(altura, 0, -1):  # Desde la altura hasta 1
    print("*" * i)  # Imprime i asteriscos
```

#### Ejemplo de Ejecución:
Si introduces `altura = 5`, el resultado será:
```
*****
****
***
**
*
```

---

### Ejercicio 4: Triángulo Invertido a la Derecha

**Objetivo**: Dibujar un triángulo invertido orientado hacia la derecha.

#### Código:
```python
# Triángulo invertido orientado hacia la derecha
altura = int(input("Introduce la altura del triángulo: "))

for i in range(altura, 0, -1):  # Desde la altura hasta 1
    print(" " * (altura - i) + "*" * i)  # Espacios + Asteriscos
```

#### Ejemplo de Ejecución:
Si introduces `altura = 5`, el resultado será:
```
*****
 ****
  ***
   **
    *
```

---

### Ejercicio 5: Rombo con Asteriscos

**Objetivo**: Dibujar un rombo utilizando dos bucles: uno para la parte superior y otro para la inferior.

#### Código:
```python
# Rombo con asteriscos
altura = int(input("Introduce la altura del rombo (debe ser un número impar): "))

# Parte superior del rombo
for i in range(1, altura + 1, 2):  # Incremento de 2 para líneas impares
    espacios = " " * ((altura - i) // 2)
    print(espacios + "*" * i)

# Parte inferior del rombo
for i in range(altura - 2, 0, -2):  # Decremento de 2 para líneas impares
    espacios = " " * ((altura - i) // 2)
    print(espacios + "*" * i)
```

#### Ejemplo de Ejecución:
Si introduces `altura = 5`, el resultado será:
```
  *
 ***
*****
 ***
  *
```
