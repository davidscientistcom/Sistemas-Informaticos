### **Ejercicios resueltos**

#### **1. Calcular el promedio de una lista de números**
```python
def promedio(lista):
    return sum(lista) / len(lista)

print(promedio([4, 8, 15, 16, 23, 42]))  # 18.0
```



#### **2. Contar cuántas palabras tiene una cadena**
```python
def contar_palabras(cadena):
    palabras = cadena.split()
    return len(palabras)

print(contar_palabras("Hola, esto es un ejemplo de prueba."))  # 7
```



#### **3. Contar las vocales y consonantes de una cadena**
```python
def contar_vocales_y_consonantes(cadena):
    vocales = "aeiouAEIOU"
    num_vocales = sum(1 for c in cadena if c in vocales)
    num_consonantes = sum(1 for c in cadena if c.isalpha() and c not in vocales)
    return num_vocales, num_consonantes

print(contar_vocales_y_consonantes("Hola Mundo"))  # (4, 5)
```



#### **4. Invertir una cadena**
```python
def invertir_cadena(cadena):
    return cadena[::-1]

print(invertir_cadena("Python"))  # nohtyP
```



#### **5. Encontrar el número más grande y más pequeño de una lista**
```python
def maximo_minimo(lista):
    return max(lista), min(lista)

print(maximo_minimo([4, 8, 15, 16, 23, 42]))  # (42, 4)
```



#### **6. Eliminar elementos duplicados de una lista**
```python
def eliminar_duplicados(lista):
    return list(set(lista))

print(eliminar_duplicados([1, 2, 2, 3, 4, 4, 5]))  # [1, 2, 3, 4, 5]
```



#### **7. Reemplazar todas las vocales en una cadena con un carácter**
```python
def reemplazar_vocales(cadena, simbolo):
    vocales = "aeiouAEIOU"
    return "".join(simbolo if c in vocales else c for c in cadena)

print(reemplazar_vocales("Hola Mundo", "*"))  # H*l* M*nd*
```



#### **8. Ordenar una lista en orden inverso**
```python
def ordenar_inverso(lista):
    return sorted(lista, reverse=True)

print(ordenar_inverso([4, 8, 15, 16, 23, 42]))  # [42, 23, 16, 15, 8, 4]
```



#### **9. Calcular cuántos números son pares e impares en una lista**
```python
def contar_pares_impares(lista):
    pares = sum(1 for n in lista if n % 2 == 0)
    impares = len(lista) - pares
    return pares, impares

print(contar_pares_impares([1, 2, 3, 4, 5, 6]))  # (3, 3)
```



#### **10. Contar cuántos números están en un rango**
```python
def contar_en_rango(lista, inicio, fin):
    return sum(1 for n in lista if inicio <= n <= fin)

print(contar_en_rango([1, 5, 10, 15, 20], 5, 15))  # 3
```



#### **11. Convertir una lista de números en una cadena separada por comas**
```python
def lista_a_cadena(lista):
    return ", ".join(str(n) for n in lista)

print(lista_a_cadena([1, 2, 3, 4]))  # "1, 2, 3, 4"
```



#### **12. Verificar si una cadena es un palíndromo**
```python
def es_palindromo(cadena):
    cadena = cadena.replace(" ", "").lower()
    return cadena == cadena[::-1]

print(es_palindromo("Anita lava la tina"))  # True
print(es_palindromo("Python"))  # False
```



#### **13. Crear una lista con los cuadrados de los números de otra lista**
```python
def cuadrados(lista):
    return [n**2 for n in lista]

print(cuadrados([1, 2, 3, 4]))  # [1, 4, 9, 16]
```



#### **14. Contar los números que son múltiplos de un valor dado**
```python
def contar_multiplos(lista, multiplo):
    return sum(1 for n in lista if n % multiplo == 0)

print(contar_multiplos([1, 2, 3, 4, 5, 6, 7, 8, 9], 3))  # 3
```



#### **15. Calcular la suma de las cifras de un número**
```python
def suma_cifras(numero):
    return sum(int(cifra) for cifra in str(numero))

print(suma_cifras(12345))  # 15
```

