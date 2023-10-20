## Ficheros y directorios

Abre una terminal en tu servidor Ubuntu.

Crea un directorio llamado "EjercicioDirectorios" en el directorio de inicio de tu usuario utilizando el comando mkdir.

Ingresa al directorio "EjercicioDirectorios" con el comando cd.

1. Crea tres directorios personalizados, por ejemplo, "Proyectos", "Backups" y "Archivos". Utiliza el comando mkdir para crear cada uno de ellos.

2. Dentro del directorio "Proyectos", crea un archivo llamado "plan.txt" utilizando el comando touch.

3. Copia el archivo "plan.txt" al directorio "Backups" utilizando el comando cp.

4. Cambia tu directorio de trabajo al directorio "Archivos" con el comando cd.

5. Crea un archivo llamado "datos.xlsx" en el directorio "Archivos" utilizando el comando touch.

6. Regresa al directorio "EjercicioDirectorios" utilizando el comando cd.

7. Lista el contenido del directorio "EjercicioDirectorios" con el comando ls. Deberías ver los tres directorios personalizados ("Proyectos", "Backups" y "Archivos") y el archivo "plan.txt".

8. Elimina el archivo "plan.txt" del directorio "Proyectos" con el comando rm.

9. Elimina el directorio "Backups" y su contenido de forma recursiva utilizando el comando rm con la opción -r.

10. Lista nuevamente el contenido del directorio "EjercicioDirectorios" para verificar que los cambios se hayan realizado correctamente.

11. Finalmente, muestra la ruta completa de tu directorio actual con el comando pwd para confirmar que estás en el directorio "EjercicioDirectorios".

## Las rutas

1. Abre una terminal en tu servidor Ubuntu.

2. Asegúrate de estar en el directorio de inicio de tu usuario. Puedes utilizar el comando cd sin argumentos para ir al directorio de inicio.

3. Crea un directorio llamado "EjercicioRutas" en tu directorio de inicio utilizando el comando mkdir.

4. Ingresa al directorio "EjercicioRutas" con el comando cd.

5. Dentro del directorio "EjercicioRutas", crea un directorio llamado "Documentos" con el comando mkdir.

6. Crea un archivo llamado "tarea.txt" dentro del directorio "Documentos" utilizando el comando touch.

7. Muestra la ruta absoluta del archivo "tarea.txt" utilizando el comando pwd. Debería verse algo similar a /ruta/al/directorio/EjercicioRutas/Documentos/tarea.txt.

8. Ahora, sal del directorio "Documentos" y regresa al directorio "EjercicioRutas" utilizando una ruta relativa con el comando cd ...

9. Utiliza el comando ls para verificar que has regresado al directorio "EjercicioRutas".

10. Regresa al directorio de inicio de tu usuario utilizando el atajo "~" con el comando cd ~.

11. Crea un archivo llamado "notas.txt" en el directorio de inicio utilizando el atajo "~" y el comando touch.

12. Cambia al directorio "Documentos" dentro del directorio "EjercicioRutas" utilizando una ruta relativa con el comando cd Documentos.

13. Muestra la ruta absoluta del directorio "Documentos" utilizando el comando pwd.

14. Accede al directorio de inicio de tu usuario desde el directorio "Documentos" utilizando una ruta absoluta con el comando cd ~/...

15. Verifica que estás en el directorio de inicio de tu usuario utilizando el comando pwd.


## LS
Asegúrate de estar en el directorio "EjercicioDirectorios" o "EjercicioRutas" según el ejercicio que estés realizando.

1. Utiliza el comando ls para listar el contenido del directorio actual. Observa los nombres de archivos y directorios.

2. Ejecuta el comando ls -l para obtener una lista detallada que incluye permisos, propietario, grupo, tamaño, fecha y nombre de los archivos y directorios.

3. Ejecuta el comando ls -a para listar archivos ocultos en el directorio (aquellos que comienzan con un punto, como ".config" o ".bashrc").

4. Ejecuta el comando ls -h para mostrar tamaños legibles por humanos en lugar de bytes, lo que facilita la comprensión de los tamaños de los archivos y directorios.

5. Utiliza el comando ls -t para listar los archivos y directorios ordenados por fecha y hora de modificación, mostrando primero los más recientes.

6. Ejecuta el comando ls -r para listar el contenido en orden inverso, es decir, desde el último archivo o directorio al primero.

7. Utiliza el comando ls -S para listar los archivos y directorios ordenados por tamaño, mostrando primero los más grandes.

8. Ejecuta el comando ls -lh para obtener una lista detallada con tamaños legibles por humanos.

9. Utiliza el comando ls -lt para listar los archivos y directorios ordenados por fecha y hora de modificación, con tamaños legibles por humanos y mostrando primero los más recientes.

10. Ejecuta el comando ls -A para listar archivos y directorios sin mostrar "." y ".." (el directorio actual y el directorio padre).

## Cambio de nombres

Asegúrate de estar en el directorio "EjercicioDirectorios" o "EjercicioRutas" según el ejercicio que estés realizando.

1. Utiliza el comando ls para listar el contenido del directorio actual y confirma los nombres de los archivos y directorios.

2. Cambia el nombre del archivo "tarea.txt" en el directorio "Documentos" al nuevo nombre "proyecto.txt" utilizando el comando mv tarea.txt proyecto.txt.

3. Ejecuta nuevamente ls para verificar que el nombre del archivo "tarea.txt" ha sido cambiado a "proyecto.txt".

4. Cambia el nombre del directorio "Backups" al nuevo nombre "CopiasDeSeguridad" utilizando el comando mv Backups CopiasDeSeguridad.

5. Utiliza ls para confirmar que el nombre del directorio "Backups" se ha cambiado a "CopiasDeSeguridad".

6. Cambia el nombre del archivo "notas.txt" en el directorio de inicio al nuevo nombre "apuntes.txt" utilizando el comando mv ~/notas.txt ~/apuntes.txt.

7. Ejecuta ls para verificar que el nombre del archivo "notas.txt" en el directorio de inicio ha sido cambiado a "apuntes.txt".

8. Cambia el nombre del directorio "Archivos" al nuevo nombre "Documentación" utilizando el comando mv Archivos Documentación.

9. Utiliza ls para confirmar que el nombre del directorio "Archivos" se ha cambiado a "Documentación".

## Patrones Simples
Asegúrate de estar en el directorio "EjercicioDirectorios" o "EjercicioRutas" según el ejercicio que estés realizando.

1. Utiliza el comando ls para listar el contenido del directorio actual y confirma los nombres de los archivos y directorios.

2. Usa el comodín * para listar todos los archivos en el directorio "Documentos" que tengan la extensión ".txt". Ejecuta el comando ls Documentos/*.txt.

3. Utiliza el comodín * para listar todos los archivos en el directorio "Documentos" que comiencen con la letra "p". Ejecuta el comando ls Documentos/p*.

4. Emplea el comodín ? para listar todos los archivos en el directorio "Documentos" que tengan un nombre de archivo de cinco letras, donde el tercer carácter sea una vocal. Ejecuta el comando ls Documentos/??[aeiou]?.*.

5. Usa el comodín * para listar todos los archivos en el directorio de inicio que tengan la extensión ".txt". Ejecuta el comando ls ~/EjercicioRutas/*.txt.

6. Utiliza el comodín * para listar todos los archivos en el directorio "Documentación" que comiencen con la letra "a". Ejecuta el comando ls Documentación/a*.

7. Emplea el comodín ? para listar todos los archivos en el directorio "Documentación" que tengan un nombre de archivo de tres letras y terminen con ".xlsx". Ejecuta el comando ls Documentación/???.xlsx.
