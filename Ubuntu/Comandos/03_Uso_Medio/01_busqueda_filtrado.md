### 1.7 Comandos Avanzados de Búsqueda y Filtrado: `grep` y `find`

#### `grep`: Búsqueda de Texto en Archivos

El comando `grep` busca patrones de texto dentro de archivos o en la salida de otros comandos. Es muy útil para encontrar información específica en archivos de texto o para filtrar líneas en la salida de comandos.

##### Ejemplos Básicos y Avanzados con `grep`

1. **Buscar una Palabra Específica en un Archivo**  
   ```bash
   grep "error" /var/log/syslog
   ```
   Busca la palabra `error` en el archivo `syslog` y muestra todas las líneas donde aparece.

2. **Búsqueda sin Distinguir entre Mayúsculas y Minúsculas**  
   ```bash
   grep -i "error" /var/log/syslog
   ```
   Con `-i`, `grep` busca sin diferenciar mayúsculas de minúsculas, mostrando coincidencias de `error`, `Error`, `ERROR`, etc.

3. **Buscar Líneas que No Contengan el Patrón**  
   ```bash
   grep -v "info" /var/log/syslog
   ```
   Con `-v`, `grep` muestra todas las líneas que **no** contienen la palabra `info`.

4. **Mostrar Número de Línea con la Coincidencia**  
   ```bash
   grep -n "warning" /var/log/syslog
   ```
   Con `-n`, `grep` incluye el número de línea en cada coincidencia, útil para identificar la ubicación exacta en el archivo.

5. **Buscar Palabras Completas**  
   ```bash
   grep -w "failed" /var/log/auth.log
   ```
   La opción `-w` busca palabras completas, es decir, evita coincidir con subcadenas, como `failed` en `user_failed_login`.

6. **Buscar en Múltiples Archivos**  
   ```bash
   grep "timeout" /var/log/*.log
   ```
   Busca la palabra `timeout` en todos los archivos con extensión `.log` en `/var/log`.

##### Combinación de `grep` con Tuberías para Filtrar Resultados

1. **Buscar un Patrón en la Salida de un Comando**  
   ```bash
   dmesg | grep "error"
   ```
   Filtra los mensajes del sistema (`dmesg`) para mostrar solo aquellos que contienen la palabra `error`.

2. **Filtrar por Múltiples Criterios con Piping y `grep`**  
   ```bash
   ps aux | grep "apache" | grep -v "grep"
   ```
   Muestra los procesos que contienen `apache` en su nombre, excluyendo la propia instancia de `grep` en la salida. Este es un método común para verificar procesos específicos en ejecución.

3. **Contar la Cantidad de Coincidencias**  
   ```bash
   ps aux | grep -c "apache"
   ```
   Con `-c`, `grep` cuenta el número de líneas que coinciden, útil para saber cuántos procesos de `apache` están en ejecución.

4. **Buscar y Resaltar Patrones Múltiples**  
   ```bash
   grep -E "error|failed|timeout" /var/log/syslog
   ```
   Utilizando `-E`, `grep` permite buscar patrones múltiples usando expresiones regulares, como `error`, `failed` y `timeout` al mismo tiempo.

#### `find`: Búsqueda de Archivos y Directorios

El comando `find` es una herramienta poderosa para buscar archivos y directorios en Linux basándose en criterios como el nombre, tamaño, fecha de modificación, permisos y más.

##### Ejemplos Básicos y Avanzados con `find`

1. **Buscar Archivos por Nombre**  
   ```bash
   find /home/usuario -name "notas.txt"
   ```
   Busca el archivo `notas.txt` en el directorio `/home/usuario` y sus subdirectorios.

2. **Buscar Archivos por Tipo**  
   ```bash
   find /var -type f -name "*.log"
   ```
   Encuentra todos los archivos (`-type f`) con extensión `.log` dentro de `/var`.

3. **Buscar Directorios Específicos**  
   ```bash
   find / -type d -name "backup"
   ```
   Encuentra directorios (`-type d`) llamados `backup` en todo el sistema.

4. **Buscar Archivos según Tamaño**  
   ```bash
   find /home/usuario -size +10M
   ```
   Encuentra archivos de más de 10 MB en `/home/usuario`.

5. **Buscar Archivos Modificados Recientemente**  
   ```bash
   find /etc -mtime -1
   ```
   Encuentra archivos en `/etc` que se modificaron en las últimas 24 horas. `-mtime -1` indica los archivos modificados en el último día.

6. **Buscar Archivos Según Permisos**  
   ```bash
   find / -type f -perm 644
   ```
   Busca archivos con permisos específicos (`644`, lectura y escritura para el propietario, solo lectura para otros) en todo el sistema.

##### Combinación de `find` con Otras Herramientas usando Tuberías

1. **Eliminar Archivos Encontrados por `find`**  
   ```bash
   find /tmp -type f -name "*.tmp" -delete
   ```
   Busca y elimina (`-delete`) archivos `.tmp` en la carpeta `/tmp`. Este es un método rápido para limpiar archivos temporales.

2. **Buscar Archivos y Usar `grep` para Filtrar el Contenido**  
   ```bash
   find /var/log -type f -name "*.log" -exec grep "error" {} \;
   ```
   Encuentra archivos `.log` en `/var/log` y ejecuta `grep "error"` en cada archivo, mostrando líneas que contienen `error`.

3. **Buscar Archivos y Contar Coincidencias con `wc`**  
   ```bash
   find /home/usuario -type f -name "*.txt" | xargs grep -c "Linux"
   ```
   Este comando busca archivos `.txt` en `/home/usuario`, filtra las líneas que contienen `Linux` y cuenta las coincidencias con `wc`.

4. **Buscar Archivos Modificados Recientemente y Guardar la Lista en un Archivo**  
   ```bash
   find /home/usuario -type f -mtime -7 > archivos_recientes.txt
   ```
   Busca archivos en `/home/usuario` modificados en los últimos 7 días y guarda la lista en `archivos_recientes.txt`.

#### Ejemplos Combinados y Avanzados con `grep` y `find`

1. **Buscar Archivos que Contienen una Palabra Específica y Filtrar con `grep`**  
   ```bash
   find /home/usuario/proyectos -type f -name "*.txt" | xargs grep "importante"
   ```
   Encuentra todos los archivos `.txt` en `/home/usuario/proyectos` y muestra las líneas que contienen la palabra `importante`.

2. **Buscar Archivos Grandes y Revisar su Contenido**  
   ```bash
   find /var/log -type f -size +100M | xargs grep "error"
   ```
   Encuentra archivos en `/var/log` que tengan más de 100 MB y muestra las líneas que contienen la palabra `error`. Este comando es útil para buscar en archivos de registro grandes sin abrirlos.

3. **Listar y Contar Archivos Según Criterios**  
   ```bash
   find /home/usuario -type f -name "*.log" | xargs ls -lh | wc -l
   ```
   Encuentra archivos `.log` en `/home/usuario`, lista sus detalles (`ls -lh`) y cuenta cuántos archivos se encontraron con `wc -l`.

4. **Buscar Archivos, Filtrar Contenido y Guardar el Resultado**  
   ```bash
   find /var/log -type f -name "*.log" | xargs grep -i "error" > errores_en_logs.txt
   ```
   Este comando busca archivos `.log` en `/var/log`, filtra las líneas que contienen `error` sin distinguir mayúsculas y minúsculas, y guarda los resultados en `errores_en_logs.txt`.

#### Redirección y Tuberías Adicionales para Optimizar `grep` y `find`

1. **Filtrar Salida de un Proceso y Ordenar por Fecha**  
   ```bash
   ps aux | grep "apache" | sort -k 10
   ```
   Filtra los procesos que contienen `apache` y los ordena por la columna de fecha/hora (columna 10).

2. **Buscar Archivos y Ordenarlos por Tamaño**  
   ```bash
   find /home/usuario -type f -name "*.txt" -exec ls -lh {} + | sort -k 5 -h
   ```
   Encuentra archivos `.txt`, lista su tamaño en formato legible (`ls -lh`), y los ordena por tamaño (columna 5) en orden ascendente.


#### Ejemplos Adicionales y Avanzados con `grep`, `find` y Combinación de Tuberías

1. **Encontrar Archivos Grandes y Buscar un Patrón dentro de Ellos**  
   Este comando es útil cuando necesitas analizar archivos grandes en busca de errores o información crítica, sin abrir cada archivo manualmente.
   ```bash
   find /var/log -type f -size +50M | xargs grep "critical"
   ```
   Aquí, `find` busca archivos de más de 50 MB en `/var/log`, y `grep` busca la palabra `critical` dentro de cada archivo encontrado.

2. **Buscar Archivos Según Permisos y Filtrar Contenido con `grep`**  
   En algunos casos, podrías necesitar encontrar archivos con permisos específicos (por ejemplo, solo lectura para el propietario) y luego buscar un patrón en su contenido.
   ```bash
   find /home/usuario -type f -perm 400 | xargs grep "confidencial"
   ```
   Este comando encuentra archivos con permisos `400` (solo lectura para el propietario) y luego utiliza `grep` para buscar la palabra `confidencial` en ellos.

3. **Combinar `find` con `grep` para Filtrar por Fecha y Contenido Específico**  
   Si necesitas analizar archivos que se han modificado recientemente y que contienen un término específico, esta combinación es ideal.
   ```bash
   find /home/usuario/proyectos -type f -mtime -3 | xargs grep "progreso"
   ```
   En este caso, `find` busca archivos en la carpeta `proyectos` que se modificaron en los últimos 3 días, y luego `grep` filtra las líneas que contienen la palabra `progreso`.

4. **Buscar Archivos con Nombres Similares en Múltiples Directorios y Ordenarlos por Tamaño**  
   Es común encontrar múltiples versiones de archivos con nombres similares en diferentes ubicaciones. Este comando ayuda a encontrar, listar y ordenar esos archivos.
   ```bash
   find / -type f -name "informe*" -exec ls -lh {} + | sort -k 5 -h
   ```
   Aquí, `find` busca archivos cuyo nombre comienza con `informe`, `ls -lh` lista detalles como el tamaño de cada archivo, y `sort -k 5 -h` los ordena por tamaño.

5. **Buscar Archivos y Contar las Ocurrencias de una Palabra con `grep` y `wc`**  
   Este comando combina `find`, `grep` y `wc` para contar cuántas veces una palabra aparece en los archivos de un directorio específico.
   ```bash
   find /home/usuario/documentos -type f -name "*.txt" | xargs grep -o "objetivo" | wc -l
   ```
   Aquí, `grep -o "objetivo"` muestra cada coincidencia de la palabra `objetivo` en una línea separada, y `wc -l` cuenta cuántas veces aparece en total.

6. **Buscar Archivos y Crear un Archivo de Registro con Solo los Resultados Relevantes**  
   Este comando es útil cuando se quiere registrar resultados de búsqueda en un archivo específico.
   ```bash
   find /etc -type f -name "*.conf" -exec grep -i "server" {} \; > registros_servidor.txt
   ```
   Este comando busca en archivos de configuración (`*.conf`) dentro de `/etc`, y guarda en `registros_servidor.txt` las líneas que contienen la palabra `server`.

7. **Buscar Procesos, Filtrar Resultados y Guardar Información en un Archivo Temporal**  
   Para análisis de procesos específicos, este comando permite registrar procesos con nombre determinado y guardar la lista en un archivo temporal.
   ```bash
   ps aux | grep "nginx" | grep -v "grep" > /tmp/procesos_nginx.txt
   ```
   Aquí, se listan los procesos que contienen `nginx` en su nombre, se excluyen las instancias de `grep`, y la salida se guarda en el archivo temporal `/tmp/procesos_nginx.txt`.

8. **Verificar y Limpiar Archivos Temporales Creados Recientemente**  
   Esta combinación busca y elimina archivos temporales creados recientemente, ideal para limpiar archivos innecesarios.
   ```bash
   find /tmp -type f -name "*.tmp" -mtime -2 | xargs rm
   ```
   En este caso, `find` localiza archivos con extensión `.tmp` en `/tmp` que fueron modificados en los últimos 2 días, y `xargs rm` los elimina.


