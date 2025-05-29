## Minicurso de Programación en Python

## Módulo 1: Variables en Python



### ¿Qué es una variable?

Una variable es una etiqueta que usamos para guardar datos en la memoria del ordenador. Piensa en una variable como una caja con un nombre, donde guardas un valor.

**Ejemplo cotidiano:**

Si tienes una caja con una etiqueta que dice calcetines y dentro hay 4 pares, eso es como hacer:

calcetines = 4


 **Concepto clave:**
	•	Las variables en Python no necesitan declararse con tipo antes de usarse.
	•	El tipo se infiere automáticamente según el valor que le asignas.



 **Reglas para nombrar variables**
	1.	Deben comenzar por una letra o un guion bajo _.
	2.	No pueden empezar por un número.
	3.	Solo pueden contener letras, números y guiones bajos.
	4.	No pueden ser palabras reservadas del lenguaje (if, for, while, etc.).


 **Ejemplos válidos de variables**

edad = 20
nombre = "Lucía"
_temperatura = 36.5
alturaEnMetros = 1.75

 **Ejemplos NO válidos**

1edad = 20          # No puede empezar con número
nombre completo = "Lucía"  # No se permiten espacios
if = 3              # 'if' es palabra reservada




 **Tipos de datos comunes en variables**

Tipo de dato	Ejemplo	Código
Número entero	Edad, número de hijos	edad = 25
Número decimal	Temperatura, peso	peso = 72.5
Cadena	Nombre, dirección	nombre = "Lucía"
Booleano	Sí o No (True/False)	es_mayor = True




**Ejemplo práctico**

nombre = "Juan"
edad = 30
peso = 70.5
es_estudiante = True

print("Nombre:", nombre)
print("Edad:", edad)
print("Peso:", peso)
print("¿Es estudiante?:", es_estudiante)

Salida:

Nombre: Juan
Edad: 30
Peso: 70.5
¿Es estudiante?: True




**Importante**
	•	Python distingue entre mayúsculas y minúsculas.
Edad y edad son variables diferentes.
	•	Puedes cambiar el valor de una variable en cualquier momento:

edad = 20
edad = edad + 1  # Ahora edad vale 21

