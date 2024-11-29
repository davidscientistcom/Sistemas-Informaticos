### 1. **mkdir** (Make Directory)  
Crea nuevos directorios.

**Ejemplos**:
1. `mkdir nueva_carpeta` - Crea un directorio llamado `nueva_carpeta`.
2. `mkdir -p ruta/otra_carpeta` - Crea una jerarquía de directorios (si no existen).
3. `mkdir -v carpeta1 carpeta2` - Muestra mensajes al crear `carpeta1` y `carpeta2`.
4. `mkdir carpeta{1..3}` - Crea tres carpetas: `carpeta1`, `carpeta2`, `carpeta3`.
5. `mkdir $(date +%Y-%m-%d)` - Crea una carpeta con la fecha actual como nombre.
6. `mkdir -m 755 carpeta` - Crea una carpeta con permisos específicos.
7. `mkdir ~/Documentos/proyecto` - Crea la carpeta `proyecto` en `Documentos`.
8. `mkdir carpeta; cd carpeta` - Crea una carpeta y accede a ella.
9. `mkdir ../otra_ruta` - Crea un directorio en la ruta superior.
10. `mkdir -p /tmp/test/{logs,data}` - Crea directorios `logs` y `data` en `/tmp/test`.



### 2. **rm** (Remove)  
Elimina archivos o directorios.

**Ejemplos**:
1. `rm archivo.txt` - Elimina el archivo `archivo.txt`.
2. `rm -i archivo.txt` - Solicita confirmación antes de eliminar.
3. `rm -r carpeta` - Elimina el directorio `carpeta` y su contenido.
4. `rm -rf carpeta` - Elimina un directorio y su contenido sin confirmar.
5. `rm archivo?.txt` - Elimina archivos como `archivo1.txt`, `archivo2.txt`, etc.
6. `rm -v archivo.txt` - Muestra qué archivo se eliminó.
7. `find . -name "*.log" -exec rm {} \;` - Busca y elimina todos los archivos `.log`.
8. `rm -d carpeta_vacía` - Elimina un directorio vacío.
9. `rm archivo*` - Elimina todos los archivos cuyo nombre comienza con `archivo`.
10. `rm /tmp/tempfile*` - Borra archivos temporales en `/tmp`.



### 3. **echo**  
Imprime texto en la terminal o redirige salida a un archivo.

**Ejemplos**:
1. `echo "Hola Mundo"` - Muestra "Hola Mundo".
2. `echo $HOME` - Muestra el valor de la variable `HOME`.
3. `echo "Texto" > archivo.txt` - Escribe "Texto" en `archivo.txt`.
4. `echo "Texto" >> archivo.txt` - Agrega "Texto" a `archivo.txt`.
5. `echo -e "Primera\nSegunda"` - Muestra líneas separadas.
6. `echo -n "Sin salto"` - Imprime sin salto de línea.
7. `echo $(ls)` - Muestra el resultado del comando `ls`.
8. `echo $((3 + 4))` - Realiza operaciones matemáticas.
9. `echo -e "\033[1;31mTexto Rojo\033[0m"` - Muestra texto en color rojo.
10. `echo "export PATH=$PATH:/usr/local/bin" >> ~/.bashrc` - Agrega una variable al archivo de configuración.



### 4. **cd** (Change Directory)  
Navega entre directorios.

**Ejemplos**:
1. `cd /` - Cambia al directorio raíz.
2. `cd ~` - Cambia al directorio del usuario actual.
3. `cd /tmp` - Accede al directorio temporal.
4. `cd ..` - Sube al directorio superior.
5. `cd -` - Cambia al directorio anterior.
6. `cd /var/log` - Accede a la carpeta de logs del sistema.
7. `cd ~/Documentos` - Cambia al directorio `Documentos`.
8. `cd carpeta1/carpeta2` - Cambia a un subdirectorio.
9. `cd "$(dirname archivo.txt)"` - Cambia al directorio de un archivo.
10. `cd /usr && ls` - Cambia al directorio y lista su contenido.



### 5. **ls** (List Files)  
Muestra el contenido de directorios.

**Ejemplos**:
1. `ls` - Lista archivos y directorios.
2. `ls -l` - Muestra detalles en formato largo.
3. `ls -a` - Incluye archivos ocultos.
4. `ls -la` - Lista archivos ocultos con formato largo.
5. `ls -lh` - Muestra tamaños legibles para humanos.
6. `ls /etc` - Lista el contenido del directorio `/etc`.
7. `ls *.txt` - Lista archivos con extensión `.txt`.
8. `ls -R` - Lista contenido recursivamente.
9. `ls -t` - Ordena por fecha de modificación.
10. `ls -S` - Ordena por tamaño.



### 6. **chown** (Change Ownership)  
Cambia el propietario de archivos o directorios.

**Ejemplos**:
1. `chown usuario archivo.txt` - Cambia el propietario de `archivo.txt`.
2. `chown usuario:grupo archivo.txt` - Cambia propietario y grupo.
3. `sudo chown root archivo.txt` - Cambia el propietario a `root`.
4. `sudo chown -R usuario directorio` - Cambia el propietario recursivamente.
5. `chown --from=usuario1 usuario2 archivo.txt` - Cambia propietario solo si es `usuario1`.
6. `sudo chown $(whoami) archivo.txt` - Cambia el propietario al usuario actual.
7. `sudo chown :nuevo_grupo archivo.txt` - Cambia solo el grupo.
8. `sudo chown -c usuario archivo.txt` - Muestra cambios realizados.
9. `sudo chown usuario:* directorio` - Cambia todos los archivos de un directorio.
10. `sudo chown usuario:nuevo_grupo archivo*` - Cambia múltiples archivos.



### 7. **chmod** (Change Mode)  
Modifica permisos de archivos y directorios.

**Ejemplos**:
1. `chmod 644 archivo.txt` - Asigna permisos de lectura y escritura.
2. `chmod u+x script.sh` - Da permisos de ejecución al propietario.
3. `chmod -R 755 directorio` - Cambia permisos recursivamente.
4. `chmod o-w archivo.txt` - Elimina permisos de escritura para otros.
5. `chmod +x archivo` - Da permisos de ejecución.
6. `chmod g+r archivo.txt` - Da permisos de lectura al grupo.
7. `chmod 777 archivo` - Da todos los permisos a todos.
8. `chmod 600 archivo_privado` - Permisos solo para el propietario.
9. `chmod u+s ejecutable` - Configura el bit setuid.
10. `chmod --reference=otro_archivo archivo` - Copia permisos de otro archivo.



### 8. **pwd** (Print Working Directory)  
Muestra el directorio actual.

**Ejemplos**:
1. `pwd` - Muestra la ruta actual.
2. `cd /var && pwd` - Cambia de directorio y muestra la nueva ubicación.
3. `pwd > ruta_actual.txt` - Guarda la ruta en un archivo.
4. `echo $(pwd)` - Usa el directorio actual en un comando.
5. `echo "Estoy en $(pwd)"` - Incluye la ruta en un mensaje.
6. `pwd | grep var` - Verifica si la ruta contiene `var`.
7. `mkdir $(pwd)/nueva_carpeta` - Crea un directorio en la ubicación actual.
8. `cp archivo.txt $(pwd)` - Copia un archivo a la ubicación actual.
9. `ln -s $(pwd)/archivo.txt enlace.txt` - Crea un enlace simbólico.
10. `pwd && ls` - Combina comandos para mostrar la ruta y el contenido.



### 9. **Redirecciones (`>`, `>>`)**  
Controlan salida de comandos.

**Ejemplos**:
1. `ls > salida.txt` - Guarda la salida en un archivo.
2. `echo "Hola" > archivo.txt` - Sobrescribe el contenido del archivo.
3. `echo "Adiós" >> archivo.txt` - Agrega contenido al final.
4. `cat archivo1 > archivo2` - Copia contenido de un archivo a otro.
5. `grep error archivo.log > errores.txt` - Guarda líneas con "error".
6. `df -h > uso_disco.txt` - Guarda información del disco.
7. `echo $(date) >> log.txt` - Agrega la fecha al archivo `log.txt`.
8. `whoami > usuario.txt` - Guarda el nombre del usuario.
9. `ls /noexiste 2> errores.txt` - Guarda errores en un archivo.
10. `ls /tmp > salida.txt && echo "Completado"` - Ejecuta y confirma.



### 10. **grep** (Global Regular Expression Print)  
Busca patrones en archivos o salida.



**Ejemplos**:
1. `grep "error" archivo.log` - Busca "error" en el archivo.
2. `grep -i "usuario" usuarios.txt` - Ignora mayúsculas/minúsculas.
3. `grep -c "error" archivo.log` - Cuenta coincidencias.
4. `grep -v "usuario" usuarios.txt` - Excluye líneas que coinciden.
5. `ls | grep ".txt"` - Filtra archivos `.txt`.
6. `grep -l "error" *.log` - Lista archivos que contienen "error".
7. `grep -r "clave" /etc` - Busca recursivamente en un directorio.
8. `grep "^usuario" usuarios.txt` - Coincide líneas que comienzan con "usuario".
9. `grep "patrón" archivo > resultados.txt` - Guarda las coincidencias.
10. `dmesg | grep "usb"` - Filtra mensajes relacionados con USB.
