### **Ejercicios Resueltos**

1. **Determinar si un número es par o impar**
   ```python
   numero = int(input("Introduce un número: "))
   if numero % 2 == 0:
       print(f"El número {numero} es par.")
   else:
       print(f"El número {numero} es impar.")
   ```

2. **Imprimir los números del 1 al 10 usando un bucle `for`**
   ```python
   for i in range(1, 11):
       print(i)
   ```

3. **Sumar los números del 1 al 100**
   ```python
   suma = 0
   for i in range(1, 101):
       suma += i
   print(f"La suma de los números del 1 al 100 es: {suma}")
   ```

4. **Contar cuántos números negativos hay en una lista**
   ```python
   numeros = [-3, 5, -1, 7, -9, 0, 4]
   contador_negativos = 0
   for numero in numeros:
       if numero < 0:
           contador_negativos += 1
   print(f"Hay {contador_negativos} números negativos en la lista.")
   ```

5. **Simular una contraseña simple con `while`**
   ```python
   contraseña = "secreto"
   intento = ""
   while intento != contraseña:
       intento = input("Introduce la contraseña: ")
       if intento != contraseña:
           print("Contraseña incorrecta.")
   print("¡Acceso concedido!")
   ```

6. **Verificar si un número está en una lista**
   ```python
   lista = [1, 3, 5, 7, 9]
   numero = int(input("Introduce un número: "))
   if numero in lista:
       print(f"El número {numero} está en la lista.")
   else:
       print(f"El número {numero} no está en la lista.")
   ```

7. **Calcular el factorial de un número**
   ```python
   numero = int(input("Introduce un número: "))
   factorial = 1
   for i in range(1, numero + 1):
       factorial *= i
   print(f"El factorial de {numero} es: {factorial}")
   ```

8. **Imprimir los números pares del 1 al 20**
   ```python
   for i in range(1, 21):
       if i % 2 == 0:
           print(i)
   ```

9. **Contar las vocales en una cadena**
   ```python
   cadena = input("Introduce una cadena de texto: ")
   contador_vocales = 0
   for letra in cadena.lower():
       if letra in "aeiou":
           contador_vocales += 1
   print(f"La cadena contiene {contador_vocales} vocales.")
   ```

10. **Calcular la media de una lista de números**
    ```python
    numeros = [10, 20, 30, 40, 50]
    suma = 0
    for numero in numeros:
        suma += numero
    media = suma / len(numeros)
    print(f"La media de la lista es: {media}")
    ```


### **Ejercicios Propuestos**

1. Pedir al usuario un número y determinar si es positivo, negativo o cero.  
2. Imprimir los números impares del 1 al 50 usando un bucle.  
3. Pedir una lista de 5 números al usuario y contar cuántos son mayores que 10.  
4. Simular un contador que imprima los números del 10 al 1 en orden descendente.  
5. Pedir al usuario su edad y comprobar si puede votar (mayores de 18 años).  
6. Leer una lista de números y calcular cuántos son pares y cuántos impares.  
7. Hacer un programa que cuente cuántas veces aparece una letra específica en una cadena.  
8. Pedir al usuario 5 palabras y mostrarlas todas en mayúsculas.  
9. Usar un bucle para sumar solo los números positivos de una lista dada.  
10. Implementar un programa que calcule la potencia de un número dado usando multiplicaciones (sin el operador **).
