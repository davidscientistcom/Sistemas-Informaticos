#### 1. **Elevar un número al cuadrado**
```python
def al_cuadrado(numero):
    return numero ** 2

# Llamada a la función
print(al_cuadrado(4))  # 16
```



#### 2. **Convertir grados Celsius a Fahrenheit**
```python
def celsius_a_fahrenheit(celsius):
    return (celsius * 9/5) + 32

# Llamada a la función
print(celsius_a_fahrenheit(25))  # 77.0
```



#### 3. **Verificar si un número es positivo**
```python
def es_positivo(numero):
    return numero > 0

# Llamada a la función
print(es_positivo(10))  # True
print(es_positivo(-3))  # False
```



#### 4. **Multiplicar un número por tres**
```python
def multiplicar_por_tres(numero):
    return numero * 3

# Llamada a la función
print(multiplicar_por_tres(7))  # 21
```



#### 5. **Calcular la longitud de una cadena**
```python
def longitud_cadena(cadena):
    return len(cadena)

# Llamada a la función
print(longitud_cadena("Python"))  # 6
```

#### 6. **Calcular el promedio de tres números**
```python
def promedio(a, b, c):
    return (a + b + c) / 3

# Llamada a la función
print(promedio(3, 6, 9))  # 6.0
```



#### 7. **Determinar si un número es divisible por otro**
```python
def es_divisible(dividendo, divisor):
    return dividendo % divisor == 0

# Llamada a la función
print(es_divisible(10, 2))  # True
print(es_divisible(10, 3))  # False
```



#### 8. **Concatenar dos cadenas con un espacio**
```python
def concatenar_cadenas(cadena1, cadena2):
    return cadena1 + " " + cadena2

# Llamada a la función
print(concatenar_cadenas("Hola", "Mundo"))  # Hola Mundo
```


#### 9. **Calcular la distancia entre dos puntos en el plano**
```python
import math

def distancia_puntos(x1, y1, x2, y2):
    return math.sqrt((x2 - x1)**2 + (y2 - y1)**2)

# Llamada a la función
print(distancia_puntos(0, 0, 3, 4))  # 5.0
```


#### 10. **Determinar si una cadena es un palíndromo**
Busca en internet que es un palíndromo.
```python
def es_palindromo(cadena):
    cadena = cadena.replace(" ", "").lower()
    return cadena == cadena[::-1]

# Llamada a la función
print(es_palindromo("anita lava la tina"))  # True
print(es_palindromo("python"))             # False
```



#### 11. **Obtener los números pares de una lista**
```python
def numeros_pares(lista):
    return [numero for numero in lista if numero % 2 == 0]

# Llamada a la función
print(numeros_pares([1, 2, 3, 4, 5, 6]))  # [2, 4, 6]
```

Entendido. Aquí tienes **10 ejemplos con bucles `for`, condicionales `if` y algunos que trabajan con listas como parámetros**:



### **Ejemplos con `for` e `if`**

#### 12. **Imprimir los números pares de una lista**
```python
def imprimir_pares(lista):
    for numero in lista:
        if numero % 2 == 0:
            print(numero)

# Llamada a la función
imprimir_pares([1, 2, 3, 4, 5, 6])  # 2, 4, 6
```



#### 13. **Contar cuántos números son positivos en una lista**
```python
def contar_positivos(lista):
    contador = 0
    for numero in lista:
        if numero > 0:
            contador += 1
    return contador

# Llamada a la función
print(contar_positivos([-1, 2, 3, -4, 5]))  # 3
```



#### 14. **Sumar los elementos de una lista**
```python
def sumar_lista(lista):
    suma = 0
    for numero in lista:
        suma += numero
    return suma

# Llamada a la función
print(sumar_lista([1, 2, 3, 4]))  # 10
```



#### 15. **Filtrar números mayores a un límite**
```python
def filtrar_mayores(lista, limite):
    resultado = []
    for numero in lista:
        if numero > limite:
            resultado.append(numero)
    return resultado

# Llamada a la función
print(filtrar_mayores([1, 10, 15, 20], 10))  # [15, 20]
```



#### 16. **Encontrar el número más grande de una lista**
```python
def encontrar_maximo(lista):
    maximo = lista[0]
    for numero in lista:
        if numero > maximo:
            maximo = numero
    return maximo

# Llamada a la función
print(encontrar_maximo([3, 5, 7, 2, 8]))  # 8
```



#### 17. **Contar palabras cortas en una lista de cadenas**
```python
def contar_palabras_cortas(lista_cadenas, longitud_maxima):
    contador = 0
    for palabra in lista_cadenas:
        if len(palabra) <= longitud_maxima:
            contador += 1
    return contador

# Llamada a la función
print(contar_palabras_cortas(["hola", "Python", "IA", "gato"], 4))  # 3
```



#### 18. **Determinar si un número está en una lista**
```python
def existe_en_lista(lista, numero):
    for elemento in lista:
        if elemento == numero:
            return True
    return False

# Llamada a la función
print(existe_en_lista([1, 2, 3, 4], 3))  # True
print(existe_en_lista([1, 2, 3, 4], 5))  # False
```



#### 19. **Obtener los caracteres únicos de una cadena**
```python
def caracteres_unicos(cadena):
    unicos = []
    for caracter in cadena:
        if caracter not in unicos:
            unicos.append(caracter)
    return unicos

# Llamada a la función
print(caracteres_unicos("programación"))  # ['p', 'r', 'o', 'g', 'a', 'm', 'c', 'i', 'ó', 'n']
```



#### 20. **Contar cuántas veces aparece un elemento en una lista**
```python
def contar_apariciones(lista, elemento):
    contador = 0
    for item in lista:
        if item == elemento:
            contador += 1
    return contador

# Llamada a la función
print(contar_apariciones([1, 2, 3, 2, 2, 4], 2))  # 3
```



#### 21. **Eliminar los números negativos de una lista**
```python
def eliminar_negativos(lista):
    resultado = []
    for numero in lista:
        if numero >= 0:
            resultado.append(numero)
    return resultado

# Llamada a la función
print(eliminar_negativos([-1, 2, -3, 4, -5, 6]))  # [2, 4, 6]
```