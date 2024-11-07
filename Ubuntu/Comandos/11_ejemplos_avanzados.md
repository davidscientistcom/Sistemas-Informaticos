### Guía Completa para Combinaciones de `cat`, `echo` y Pipes

Antes de comenzar, vamos a crear algunos archivos y directorios que utilizaremos en estos ejemplos. Esto asegurará que los estudiantes puedan seguir todos los pasos sin problemas de configuración previa.

---

#### Preparación del Entorno de Ejemplo
Ejecuta estos comandos para crear archivos y directorios que necesitaremos en los ejemplos.

1. **Crear Directorio de Trabajo y Archivos:**
   ```bash
   mkdir ~/comandos_ejemplo
   cd ~/comandos_ejemplo
   touch archivo1.txt archivo2.txt archivo3.txt
   echo "Contenido inicial del archivo 1" > archivo1.txt
   echo "Otra línea de archivo 1" >> archivo1.txt
   echo "Contenido inicial del archivo 2" > archivo2.txt
   echo "Línea de error en el log" > archivo.log
   ```

---

### Ejemplos Básicos

1. **Mostrar Mensaje Simple con `echo`:**
   ```bash
   echo "Hola Mundo"
   ```
   Imprime “Hola Mundo” en la terminal.

2. **Mostrar Contenido de un Archivo con `cat`:**
   ```bash
   cat archivo1.txt
   ```
   Muestra el contenido del archivo `archivo1.txt` en la terminal.

3. **Usar `echo` para Crear un Archivo:**
   ```bash
   echo "Texto de ejemplo" > nuevo_archivo.txt
   ```
   Crea `nuevo_archivo.txt` con el texto “Texto de ejemplo”. Si el archivo ya existe, se sobrescribe.

4. **Concatenar Archivos con `cat`:**
   ```bash
   cat archivo1.txt archivo2.txt > archivo_combinado.txt
   ```
   Combina el contenido de `archivo1.txt` y `archivo2.txt` en un archivo nuevo llamado `archivo_combinado.txt`.

---

### Ejemplos Intermedios: Redirección y Pipes

1. **Añadir Texto a un Archivo Existente con `echo`:**
   ```bash
   echo "Nueva línea añadida" >> archivo1.txt
   cat archivo1.txt
   ```
   Añade una línea de texto al final de `archivo1.txt` y luego muestra su contenido para confirmar el cambio.

2. **Contar el Número de Líneas en un Archivo con `cat` y `wc -l`:**
   ```bash
   cat archivo1.txt | wc -l
   ```
   Usa `cat` para mostrar el contenido de `archivo1.txt` y cuenta las líneas con `wc -l`.

3. **Agregar un Encabezado a un Archivo con `echo` y `cat`:**
   ```bash
   echo "Encabezado del archivo" | cat - archivo1.txt > archivo_con_encabezado.txt
   ```
   Crea un nuevo archivo con un encabezado seguido del contenido de `archivo1.txt`.

4. **Mostrar Mensaje y Contenido de un Archivo Simultáneamente:**
   ```bash
   echo "Contenido del archivo 1:"; cat archivo1.txt
   ```
   Muestra un mensaje antes de listar el contenido de `archivo1.txt`.

5. **Concatenar Varias Líneas de Texto en un Solo Archivo:**
   ```bash
   echo -e "Primera línea\nSegunda línea" | cat - archivo1.txt > archivo_extendido.txt
   ```
   Añade “Primera línea” y “Segunda línea” al inicio de `archivo1.txt` y guarda el resultado en `archivo_extendido.txt`.

---

### Ejemplos Avanzados: Procesamiento de Texto en Tiempo Real

1. **Buscar y Mostrar Solo Líneas con un Texto Específico en un Archivo:**
   ```bash
   cat archivo.log | grep "error"
   ```
   Filtra y muestra solo las líneas que contienen la palabra “error” en `archivo.log`.

2. **Crear un Archivo Temporal con Contenido Dinámico:**
   ```bash
   echo "Inicio del archivo temporal" > temp.txt
   cat archivo1.txt >> temp.txt
   echo "Final del archivo temporal" >> temp.txt
   cat temp.txt
   ```
   Crea `temp.txt` con una primera línea fija, añade el contenido de `archivo1.txt` y finaliza con una línea adicional.

3. **Usar `echo`, `cat`, y `grep` para Filtrar y Procesar Texto:**
   ```bash
   cat archivo.log | grep "ERROR" | while read line; do echo "Error encontrado: $line"; done
   ```
   Busca errores en `archivo.log` y muestra cada línea que contiene "ERROR" con un prefijo personalizado.

4. **Capturar Entrada del Usuario y Guardarla en un Archivo:**
   ```bash
   echo "Escribe un mensaje:"; read mensaje; echo "$mensaje" | cat - archivo1.txt > archivo_con_mensaje.txt
   ```
   Solicita un mensaje al usuario, lo guarda antes del contenido de `archivo1.txt` en un archivo nuevo.

5. **Comando Complejo para Procesar y Ordenar Datos:**
   ```bash
   cat archivo1.txt archivo2.txt | sort | uniq > archivo_ordenado_unico.txt
   ```
   Combina el contenido de `archivo1.txt` y `archivo2.txt`, lo ordena y elimina duplicados, y guarda el resultado en `archivo_ordenado_unico.txt`.

---

### Ejercicio Práctico para Estudiantes

Para reforzar estos conceptos, los estudiantes pueden realizar el siguiente ejercicio práctico:

1. **Crear un Nuevo Directorio y Archivos para el Ejercicio:**
   ```bash
   mkdir ~/ejercicio_final
   cd ~/ejercicio_final
   touch reporte1.log reporte2.log reporte3.log
   echo "INFO: Sistema inicializado" > reporte1.log
   echo "ERROR: No se pudo conectar a la base de datos" > reporte2.log
   echo "WARNING: Espacio en disco bajo" > reporte3.log
   ```

2. **Ejercicios Específicos de Combinación de Comandos:**
   - Usar `cat` y `grep` para encontrar errores en todos los archivos `.log`.
     ```bash
     cat reporte*.log | grep "ERROR"
     ```
   - Crear un archivo `resumen.txt` con un encabezado, seguido de todos los mensajes de advertencia y error.
     ```bash
     echo "Resumen de Errores y Advertencias:" > resumen.txt
     cat reporte*.log | grep -E "ERROR|WARNING" >> resumen.txt
     ```
   - Contar el número de líneas de advertencia en cada archivo `.log`.
     ```bash
     for file in reporte*.log; do echo "$file tiene"; grep -c "WARNING" $file; done
     ```
