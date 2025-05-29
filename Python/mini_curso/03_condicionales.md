## Módulo 3: Condicionales en Python

## ¿Qué es una condicional?

Una condicional permite que un programa tome decisiones. Según si se cumple una condición o no, el programa ejecutará una parte del código u otra.

Piénsalo como un “si pasa esto, haz esto otro”.

Sintaxis de if

if condición:
    # Bloque de código si se cumple la condición

Ejemplo básico

edad = 18

if edad >= 18:
    print("Eres mayor de edad")

📌 Importante: La sangría (indentación) en Python es obligatoria. Normalmente es de 4 espacios.


 **Agregando else**

else se ejecuta si la condición del if NO se cumple.

edad = 15

if edad >= 18:
    print("Eres mayor de edad")
else:
    print("Eres menor de edad")


**Agregando elif (else if)**

Usamos elif si queremos probar más de una condición.

nota = 7

if nota >= 9:
    print("Sobresaliente")
elif nota >= 7:
    print("Notable")
elif nota >= 5:
    print("Aprobado")
else:
    print("Suspenso")



 **Condiciones múltiples**

Usando and, or, not

edad = 20
tiene_carnet = True

if edad >= 18 and tiene_carnet:
    print("Puedes conducir")




⚠️ Dudas típicas

❓¿Puedo usar varios if seguidos?

Sí, pero no es lo mismo que usar elif.

nota = 10

if nota >= 5:
    print("Aprobado")
if nota >= 9:
    print("Excelente")

Este código imprimirá dos frases si la nota es 10.

En cambio, con elif se detiene en la primera que se cumple:

if nota >= 9:
    print("Excelente")
elif nota >= 5:
    print("Aprobado")

Solo imprimirá una frase.



**Ejemplo completo**

usuario = "admin"
contraseña = "1234"

if usuario == "admin" and contraseña == "1234":
    print("Acceso concedido")
else:
    print("Usuario o contraseña incorrectos")




 Ejercicios propuestos
	1.	Pide una edad al usuario y muestra si puede votar (mayor o igual a 18).
	2.	Pide una nota y clasifícala como:
	•	Sobresaliente (≥9)
	•	Notable (≥7)
	•	Aprobado (≥5)
	•	Suspenso (<5)
	3.	Pide dos números y di cuál es mayor, menor o si son iguales.