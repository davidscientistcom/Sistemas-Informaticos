## Módulo 2: Operadores sobre Variables en Python



 ### ¿Qué es un operador?

Un operador es un símbolo que se usa para realizar una operación entre una o más variables o valores. Por ejemplo, + suma, * multiplica, etc.



### Tipos de operadores en Python

Vamos a ver los más usados:

1. Operadores Aritméticos

Sirven para hacer operaciones matemáticas.

Operador	Nombre	Ejemplo	Resultado
+	Suma	3 + 2	5
-	Resta	5 - 1	4
*	Multiplicación	4 * 2	8
/	División real	6 / 2	3.0
//	División entera	7 // 2	3
%	Módulo (resto)	7 % 2	1
**	Potencia	3 ** 2	9




**Ejemplo completo**

a = 10
b = 3

print("Suma:", a + b)
print("Resta:", a - b)
print("Multiplicación:", a * b)
print("División:", a / b)
print("División entera:", a // b)
print("Resto:", a % b)
print("Potencia:", a ** b)




2. Operadores de Asignación

Usados para dar o actualizar valores a variables.

Operador	Ejemplo	Equivale a
=	x = 5	Asigna 5 a x
+=	x += 2	x = x + 2
-=	x -= 1	x = x - 1
*=	x *= 3	x = x * 3
/=	x /= 2	x = x / 2
**=	x **= 2	x = x ** 2



 **Ejemplo práctico de asignación**

x = 10
print("Valor inicial:", x)

x += 5
print("Después de sumar 5:", x)

x *= 2
print("Después de multiplicar por 2:", x)




3. Operadores Relacionales (Comparación)

Comparan valores y devuelven True o False (booleano).

Operador	Significado	Ejemplo	Resultado
==	Igual a	5 == 5	True
!=	Distinto de	5 != 3	True
<	Menor que	3 < 5	True
>	Mayor que	5 > 3	True
<=	Menor o igual que	3 <= 3	True
>=	Mayor o igual que	4 >= 5	False




**Ejemplo con comparación**

edad = 18
print("¿Eres mayor de edad?", edad >= 18)  # True




4. Operadores Lógicos

Trabajan con valores booleanos (True, False).

Operador	Significado	Ejemplo	Resultado
and	Y lógico	True and False	False
or	O lógico	True or False	True
not	Negación	not True	False


**Ejemplo con lógica**

mayor_edad = True
tiene_dni = False

print("¿Puede votar?", mayor_edad and tiene_dni)
print("¿Tiene algún documento?", mayor_edad or tiene_dni)
print("¿No tiene DNI?", not tiene_dni)


**Pregunta Frecuente.**

- ¿Puedo sumar una cadena de texto y un número?

❌ No directamente. Eso daría error.

✅ Tienes que convertir el número a cadena:

nombre = "Lucía"
edad = 20
print("Hola, me llamo " + nombre + " y tengo " + str(edad) + " años.")
