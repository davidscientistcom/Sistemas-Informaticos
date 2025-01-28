# Ejemplo 1: Leer un archivo completo
with open("archivo1.txt", "r", encoding="utf-8") as f:
    contenido = f.read()
    print(contenido)

# Ejemplo 2: Escribir texto en un archivo (sobrescribiendo si existe)
with open("archivo2.txt", "w", encoding="utf-8") as f:
    f.write("Línea de texto.\nOtra línea.\n")

# Ejemplo 3: Añadir texto al final de un archivo
with open("archivo3.txt", "a", encoding="utf-8") as f:
    f.write("Texto adicional.\n")

# Ejemplo 4: Leer línea por línea en un bucle
with open("archivo4.txt", "r", encoding="utf-8") as f:
    for linea in f:
        print(linea.strip())

# Ejemplo 5: Leer todas las líneas en una lista
with open("archivo5.txt", "r", encoding="utf-8") as f:
    lineas = f.readlines()
    print(lineas)

# Ejemplo 6: Escribir varias líneas desde una lista
lista_lineas = ["Primera línea\n", "Segunda línea\n", "Tercera línea\n"]
with open("archivo6.txt", "w", encoding="utf-8") as f:
    f.writelines(lista_lineas)

# Ejemplo 7: Leer una parte específica (bytes)
with open("archivo7.txt", "r", encoding="utf-8") as f:
    parcial = f.read(10)
    print(parcial)

# Ejemplo 8: Escribir valores en un archivo usando format
texto = "Valor: {}"
numero = 123
with open("archivo8.txt", "w", encoding="utf-8") as f:
    f.write(texto.format(numero))

# Ejemplo 9: Crear o sobrescribir un archivo con una sola línea
with open("archivo9.txt", "w", encoding="utf-8") as f:
    f.write("Este es un nuevo contenido.\n")

# Ejemplo 10: Cambiar posición del cursor en el archivo
with open("archivo10.txt", "w+", encoding="utf-8") as f:
    f.write("1234567890")
    f.seek(5)
    f.write("XYZ")

# Ejemplo 11: Leer un archivo grande por bloques
with open("archivo11.txt", "r", encoding="utf-8") as f:
    while True:
        bloque = f.read(20)
        if not bloque:
            break
        print(bloque)

# Ejemplo 12: Guardar datos de configuración en un archivo
config = {
    "usuario": "admin",
    "puerto": 8080,
    "debug": True
}
with open("archivo12.txt", "w", encoding="utf-8") as f:
    f.write(str(config))

# Ejemplo 13: Leer y dividir líneas
with open("archivo13.txt", "r", encoding="utf-8") as f:
    for linea in f:
        partes = linea.split(",")
        print(partes)

# Ejemplo 14: Crear un archivo vacío o asegurarse de que existe
with open("archivo14.txt", "a", encoding="utf-8"):
    pass

# Ejemplo 15: Escribir texto y luego seguir escribiendo en la misma sesión
with open("archivo15.txt", "w", encoding="utf-8") as f:
    f.write("Primera parte.\n")
    f.write("Segunda parte.\n")

# Ejemplo 16: Copiar contenido de un archivo a otro
with open("archivo16_origen.txt", "r", encoding="utf-8") as origen, \
     open("archivo16_destino.txt", "w", encoding="utf-8") as destino:
    for linea in origen:
        destino.write(linea)

# Ejemplo 17: Leer sin almacenar todo en memoria (línea a línea)
with open("archivo17.txt", "r", encoding="utf-8") as f:
    linea = f.readline()
    while linea:
        print(linea.strip())
        linea = f.readline()

# Ejemplo 18: Añadir varias líneas a un archivo
nuevas_lineas = ["Línea uno\n", "Línea dos\n", "Línea tres\n"]
with open("archivo18.txt", "a", encoding="utf-8") as f:
    f.writelines(nuevas_lineas)

# Ejemplo 19: Leer un archivo binario (por ejemplo, imagen)
with open("imagen.jpg", "rb") as f:
    contenido_binario = f.read(50)
    print(contenido_binario)

# Ejemplo 20: Escribir un archivo binario
datos_binarios = b'\x00\x01\x02\x03\x04'
with open("archivo20.dat", "wb") as f:
    f.write(datos_binarios)
