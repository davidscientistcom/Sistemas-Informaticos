## **Ejercicios básicos: Variables y condicionales**

### **Ejercicios resueltos**
1. **Suma de dos números:**
   ```python
   a = 5
   b = 3
   suma = a + b
   print("La suma es:", suma)
   ```
2. **Número positivo, negativo o cero:**
   ```python
   numero = -4
   if numero > 0:
       print("Es positivo")
   elif numero < 0:
       print("Es negativo")
   else:
       print("Es cero")
   ```
3. **Comparar dos números:**
   ```python
   a = 7
   b = 10
   if a > b:
       print("A es mayor que B")
   elif a < b:
       print("A es menor que B")
   else:
       print("A es igual a B")
   ```
4. **Verificar si un número es par o impar:**
   ```python
   numero = 8
   if numero % 2 == 0:
       print("Es par")
   else:
       print("Es impar")
   ```
5. **Calcular el área de un rectángulo:**
   ```python
   base = 5
   altura = 3
   area = base * altura
   print("El área del rectángulo es:", area)
   ```
6. **Determinar si un número está en un rango:**
   ```python
   numero = 15
   if 10 <= numero <= 20:
       print("Está en el rango [10, 20]")
   else:
       print("No está en el rango")
   ```
7. **Calcular la edad en años:**
   ```python
   anio_nacimiento = 2000
   anio_actual = 2024
   edad = anio_actual - anio_nacimiento
   print("La edad es:", edad)
   ```
8. **Categorizar la edad:**
   ```python
   edad = 17
   if edad < 18:
       print("Es menor de edad")
   else:
       print("Es mayor de edad")
   ```
9. **Descuento en una tienda:**
   ```python
   precio = 100
   descuento = 0.2 if precio > 50 else 0.1
   precio_final = precio - (precio * descuento)
   print("Precio final:", precio_final)
   ```
10. **Determinar si un carácter es una vocal:**
    ```python
    caracter = 'a'
    if caracter in 'aeiouAEIOU':
        print("Es una vocal")
    else:
        print("No es una vocal")
    ```



## **Ejercicios básicos: Bucles for**

### **Ejercicios resueltos**
1. **Imprimir números del 1 al 10:**
   ```python
   for i in range(1, 11):
       print(i)
   ```
2. **Sumar los números del 1 al 100:**
   ```python
   suma = 0
   for i in range(1, 101):
       suma += i
   print("La suma es:", suma)
   ```
3. **Tabla de multiplicar del 5:**
   ```python
   for i in range(1, 11):
       print(f"5 x {i} = {5 * i}")
   ```
4. **Imprimir una lista:**
   ```python
   frutas = ["manzana", "plátano", "naranja"]
   for fruta in frutas:
       print(fruta)
   ```
5. **Contar vocales en una palabra:**
   ```python
   palabra = "Python"
   contador = 0
   for letra in palabra:
       if letra in 'aeiouAEIOU':
           contador += 1
   print("Número de vocales:", contador)
   ```
6. **Generar una lista con cuadrados de números:**
   ```python
   cuadrados = []
   for i in range(1, 6):
       cuadrados.append(i**2)
   print(cuadrados)
   ```
7. **Contar números pares en un rango:**
   ```python
   contador = 0
   for i in range(1, 21):
       if i % 2 == 0:
           contador += 1
   print("Hay", contador, "números pares")
   ```
8. **Invertir una cadena:**
   ```python
   cadena = "Python"
   invertida = ""
   for letra in cadena:
       invertida = letra + invertida
   print("Cadena invertida:", invertida)
   ```
9. **Imprimir números impares entre 10 y 20:**
   ```python
   for i in range(10, 21):
       if i % 2 != 0:
           print(i)
   ```
10. **Filtrar números mayores de una lista:**
    ```python
    numeros = [3, 8, 12, 5, 9]
    for numero in numeros:
        if numero > 5:
            print(numero)
    ```



## **Ejercicios básicos: Bucles while**

### **Ejercicios resueltos**
1. **Contar hasta 10:**
   ```python
   i = 1
   while i <= 10:
       print(i)
       i += 1
   ```
2. **Sumar números hasta que la suma sea mayor que 50:**
   ```python
   suma = 0
   i = 1
   while suma <= 50:
       suma += i
       i += 1
   print("La suma final es:", suma)
   ```
3. **Adivinar un número:**
   ```python
   secreto = 7
   intento = 0
   while intento != secreto:
       intento = int(input("Adivina el número: "))
   print("¡Correcto!")
   ```
4. **Imprimir números pares hasta 20:**
   ```python
   i = 2
   while i <= 20:
       print(i)
       i += 2
   ```
5. **Contar vocales en una palabra:**
   ```python
   palabra = "Python"
   i = 0
   contador = 0
   while i < len(palabra):
       if palabra[i] in 'aeiouAEIOU':
           contador += 1
       i += 1
   print("Número de vocales:", contador)
   ```
6. **Salir de un bucle con una condición:**
   ```python
   i = 1
   while True:
       print(i)
       i += 1
       if i > 5:
           break
   ```
7. **Crear una lista con números del 1 al 10:**
   ```python
   i = 1
   numeros = []
   while i <= 10:
       numeros.append(i)
       i += 1
   print(numeros)
   ```
8. **Imprimir números en orden descendente:**
   ```python
   i = 10
   while i > 0:
       print(i)
       i -= 1
   ```
9. **Calcular el factorial de un número:**
   ```python
   numero = 5
   factorial = 1
   i = 1
   while i <= numero:
       factorial *= i
       i += 1
   print("El factorial es:", factorial)
   ```
10. **Adivinar una palabra secreta:**
    ```python
    secreta = "python"
    intento = ""
    while intento != secreta:
        intento = input("Adivina la palabra: ")
    print("¡Correcto!")
    ```



## **Ejercicios para resolver**

### **Variables y condicionales**
1. Modifica el programa de comparar números para que también calcule la diferencia entre ellos.
2. Cambia el programa de descuento para incluir tres niveles de descuento: 10%, 20%, y 30%.
3. Usa el ejercicio de números pares e impares para también verificar si el número es divisible entre 5.
4. Modifica el cálculo del área para pedir el valor de base y altura al usuario.
5. Haz que el programa de vocales funcione con palabras completas.
6. Calcula el promedio de tres números ingresados por el usuario.
7. Determina si un número ingresado es divisible entre 3 y 5 al mismo tiempo.
8. Verifica si un año ingresado es bisiesto.
9. Convierte una temperatura de Celsius a Fahrenheit.
10. Escribe un programa para determinar si un triángulo es equilátero, isósceles o escaleno.

### **Bucles for**
1. Genera una tabla de multiplicar para un número ingresado por el usuario.
2. Modifica el programa de cuadrados para calcular también cubos.
3. Filtra números menores a 10 en una lista proporcionada por el usuario.
4. Genera una lista de números impares en un rango definido por el usuario.
5. Cuenta las consonantes en una palabra ingresada por el usuario.
6. Suma los números en una lista hasta que la suma sea mayor a 50.
7. Imprime los números de una lista en orden inverso.
8. Encuentra el número mayor en una lista de números.
9. Haz un programa que genere una lista con los primeros 10 números primos.
10. Crea un programa

 que encuentre los múltiplos de 3 en un rango.

### **Bucles while**
1. Haz un programa que sume números hasta que el usuario ingrese 0.
2. Modifica el programa de adivinar el número para que dé pistas de si es mayor o menor.
3. Crea un programa que permita ingresar palabras hasta que se escriba "salir".
4. Calcula la suma de los dígitos de un número ingresado por el usuario.
5. Haz un programa que imprima números hasta que el usuario indique que quiere detenerse.
6. Modifica el programa del factorial para que sea más interactivo.
7. Haz un programa que convierta un número binario a decimal.
8. Cuenta las palabras de una frase ingresada por el usuario.
9. Escribe un programa que permita ingresar números hasta que se ingrese un número negativo.
10. Calcula la suma y promedio de números hasta que el usuario ingrese "fin". 
