## Ejercicio 1: Cambiar el propietario de un archivo y ajustar permisos

**Objetivo:** Cambiar el propietario de un archivo y establecer permisos específicos.

Asegúrate de estar en el directorio "EjercicioDirectorios" o "EjercicioRutas" según corresponda.

1. Utiliza el comando ls -l para listar los archivos y directorios en el directorio actual. Observa quién es el propietario de los archivos.

2. Cambia el propietario del archivo "apuntes.txt" en el directorio de inicio al usuario "nuevo_usuario" utilizando chown.

Ejemplo:

``` shell
Copy code
sudo chown nuevo_usuario ~/apuntes.txt
```
3. Comprueba el cambio de propietario ejecutando ls -l nuevamente y verifica que "apuntes.txt" ahora tiene a "nuevo_usuario" como propietario.

4.  Cambia los permisos del archivo "proyecto.txt" en el directorio "Documentos" para que solo el propietario pueda escribir y ejecutar el archivo. Los permisos numéricos serían 710.

Ejemplo:
``` shell
Copy code
chmod 710 Documentos/proyecto.txt
```
5. Intenta abrir y modificar "proyecto.txt" y verifica que solo el propietario puede hacerlo.

## Ejercicio 2: Cambiar el grupo de un directorio y ajustar permisos

**Objetivo**: Cambiar el grupo de un directorio y establecer permisos específicos.

1. Asegúrate de estar en el directorio "EjercicioDirectorios" o "EjercicioRutas" según corresponda.

2. Utiliza el comando ls -l para listar los archivos y directorios en el directorio actual. Observa quién es el propietario y el grupo de los archivos.

3. Cambia el grupo del directorio "CopiasDeSeguridad" al grupo "nuevo_grupo" utilizando chown.

Ejemplo:

``` shell
Copy code
sudo chown :nuevo_grupo CopiasDeSeguridad
```
4. Comprueba el cambio de grupo ejecutando ls -l nuevamente y verifica que el directorio "CopiasDeSeguridad" ahora tiene "nuevo_grupo" como su grupo asociado.