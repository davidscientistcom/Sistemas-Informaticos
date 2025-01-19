### **Cadenas en Python**

Las **cadenas** (o strings) son secuencias de caracteres que se utilizan para trabajar con texto. En Python, las cadenas se escriben entre comillas simples `'` o dobles `"`.

#### **Características principales de las cadenas**
1. **Inmutables**: Una vez creadas, no se pueden modificar directamente.
2. **Métodos útiles**: Python tiene muchas funciones integradas para manipular cadenas.
3. **Acceso por índice**: Cada carácter en una cadena tiene un índice que empieza en `0`.



### **Ejemplos básicos de cadenas**

#### 1. Crear una cadena
```python
mi_cadena = "Hola, mundo"
print(mi_cadena)  # Hola, mundo
```

#### 2. Acceder a un carácter por su índice
```python
mi_cadena = "Python"
print(mi_cadena[0])  # P
print(mi_cadena[-1])  # n (índice negativo)
```

#### 3. Concatenar cadenas
```python
cadena1 = "Hola"
cadena2 = "Mundo"
print(cadena1 + " " + cadena2)  # Hola Mundo
```

#### 4. Subcadenas (slicing)
```python
mi_cadena = "Python"
print(mi_cadena[0:3])  # Pyt (del índice 0 al 2)
```



### **10 ejemplos de trabajo con cadenas usando funciones**

#### **Ejemplo 1: Convertir a mayúsculas (`upper`)**
```python
def convertir_mayusculas(cadena):
    return cadena.upper()

print(convertir_mayusculas("hola"))  # HOLA
```



#### **Ejemplo 2: Convertir a minúsculas (`lower`)**
```python
def convertir_minusculas(cadena):
    return cadena.lower()

print(convertir_minusculas("HOLA"))  # hola
```



#### **Ejemplo 3: Eliminar espacios al principio y al final (`strip`)**
```python
def eliminar_espacios(cadena):
    return cadena.strip()

print(eliminar_espacios("  hola mundo  "))  # "hola mundo"
```



#### **Ejemplo 4: Reemplazar una palabra o carácter (`replace`)**
```python
def reemplazar_palabra(cadena, vieja, nueva):
    return cadena.replace(vieja, nueva)

print(reemplazar_palabra("Hola, mundo", "mundo", "Python"))  # Hola, Python
```



#### **Ejemplo 5: Dividir una cadena en palabras (`split`)**
```python
def dividir_cadena(cadena):
    return cadena.split()

print(dividir_cadena("Hola mundo Python"))  # ['Hola', 'mundo', 'Python']
```



#### **Ejemplo 6: Unir una lista de palabras en una cadena (`join`)**
```python
def unir_palabras(lista, separador):
    return separador.join(lista)

print(unir_palabras(["Hola", "mundo", "Python"], " "))  # Hola mundo Python
```



#### **Ejemplo 7: Comprobar si una subcadena está presente (`in`)**
```python
def contiene_subcadena(cadena, subcadena):
    return subcadena in cadena

print(contiene_subcadena("Hola, mundo", "mundo"))  # True
print(contiene_subcadena("Hola, mundo", "Python"))  # False
```



#### **Ejemplo 8: Contar cuántas veces aparece un carácter o palabra (`count`)**
```python
def contar_apariciones(cadena, subcadena):
    return cadena.count(subcadena)

print(contar_apariciones("banana", "a"))  # 3
```



#### **Ejemplo 9: Verificar si una cadena comienza con un prefijo (`startswith`)**
```python
def empieza_con(cadena, prefijo):
    return cadena.startswith(prefijo)

print(empieza_con("Hola, mundo", "Hola"))  # True
print(empieza_con("Hola, mundo", "Mundo"))  # False
```



#### **Ejemplo 10: Invertir una cadena**
```python
def invertir_cadena(cadena):
    return cadena[::-1]

print(invertir_cadena("Python"))  # nohtyP
```



### **Resumen de métodos de cadenas utilizados**

| **Método**       | **Descripción**                                      |
|-||
| `upper`          | Convierte todos los caracteres a mayúsculas.         |
| `lower`          | Convierte todos los caracteres a minúsculas.         |
| `strip`          | Elimina espacios al principio y al final de la cadena.|
| `replace`        | Reemplaza una subcadena por otra.                    |
| `split`          | Divide una cadena en una lista según un separador.   |
| `join`           | Une una lista de cadenas en una sola con un separador.|
| `in`             | Verifica si una subcadena está presente.             |
| `count`          | Cuenta las veces que aparece una subcadena.          |
| `startswith`     | Verifica si una cadena comienza con un prefijo.      |
| `[::-1]`         | Invierte el orden de los caracteres de la cadena.    |


### **Conversión de números a cadenas y concatenación**

En Python, **números y cadenas** son tipos diferentes. Si quieres combinarlos, necesitas convertir los números a cadenas utilizando `str()`.

#### **Conversión con `str()`**
El método `str()` convierte cualquier número en una cadena de texto. Esto es útil para mostrar resultados o construir cadenas dinámicamente.

```python
numero = 123
cadena = str(numero)
print(cadena)  # "123"
```



#### **Concatenación de cadenas**
1. **Usar el operador `+`**: Une dos o más cadenas. Todos los elementos deben ser cadenas.
2. **Usar `f-strings`**: Más eficiente y legible, combina texto y variables usando `{}`.

```python
numero = 42

# Concatenación con `+`
mensaje1 = "El número es " + str(numero)
print(mensaje1)  # "El número es 42"

# Concatenación con `f-strings`
mensaje2 = f"El número es {numero}"
print(mensaje2)  # "El número es 42"
```



### **Relación entre cadenas y listas**

Las **cadenas** son similares a las listas porque:
1. Son **secuencias**: Puedes acceder a elementos con índices (`cadena[0]`).
2. Puedes usar bucles para iterar sobre ellas.
3. Algunos métodos funcionan en ambas (como `len()`).

Pero hay diferencias:
- Las cadenas son **inmutables**, mientras que las listas son **mutables**.
- Puedes dividir (`split`) o unir (`join`) cadenas con listas.



### **Ejemplo avanzado: Contar vocales, números o consonantes en una cadena**

#### **Contar vocales**
```python
def contar_vocales(cadena):
    vocales = "aeiouAEIOU"
    contador = 0
    for caracter in cadena:
        if caracter in vocales:
            contador += 1
    return contador

# Ejemplo
print(contar_vocales("Hola, Mundo"))  # 4
```



#### **Contar números**
```python
def contar_numeros(cadena):
    contador = 0
    for caracter in cadena:
        if caracter.isdigit():  # Verifica si es un dígito
            contador += 1
    return contador

# Ejemplo
print(contar_numeros("123 Hola 456"))  # 6
```



#### **Contar consonantes**
```python
def contar_consonantes(cadena):
    vocales = "aeiouAEIOU"
    contador = 0
    for caracter in cadena:
        if caracter.isalpha() and caracter not in vocales:
            contador += 1
    return contador

# Ejemplo
print(contar_consonantes("Hola, Mundo"))  # 5
```



### **Más ejemplos avanzados**

#### **Convertir una cadena a lista de caracteres**
```python
def cadena_a_lista(cadena):
    return list(cadena)

# Ejemplo
print(cadena_a_lista("Python"))  # ['P', 'y', 't', 'h', 'o', 'n']
```



#### **Unir una lista de caracteres en una cadena**
```python
def lista_a_cadena(lista):
    return "".join(lista)

# Ejemplo
print(lista_a_cadena(['P', 'y', 't', 'h', 'o', 'n']))  # "Python"
```



#### **Eliminar números de una cadena**
```python
def eliminar_numeros(cadena):
    resultado = ""
    for caracter in cadena:
        if not caracter.isdigit():  # Si no es un número
            resultado += caracter
    return resultado

# Ejemplo
print(eliminar_numeros("123Hola456"))  # "Hola"
```



#### **Reemplazar vocales con un símbolo**
```python
def reemplazar_vocales(cadena, simbolo):
    vocales = "aeiouAEIOU"
    resultado = ""
    for caracter in cadena:
        if caracter in vocales:
            resultado += simbolo
        else:
            resultado += caracter
    return resultado

# Ejemplo
print(reemplazar_vocales("Hola, Mundo", "*"))  # "H*l*, M*nd*"
```



#### **Dividir una cadena en palabras y contarlas**
```python
def contar_palabras(cadena):
    palabras = cadena.split()  # Divide por espacios
    return len(palabras)

# Ejemplo
print(contar_palabras("Hola, esto es un ejemplo"))  # 5
```
