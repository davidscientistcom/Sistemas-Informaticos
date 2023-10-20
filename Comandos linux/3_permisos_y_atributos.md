## Permisos de usuarios

En sistemas Unix y Linux, el cambio de usuario y grupo en un archivo o directorio se refiere a la acción de modificar la propiedad del archivo o directorio para que un usuario diferente y/o un grupo diferente sean responsables de él. Esto es esencial para la gestión de la seguridad y el acceso a recursos en un sistema Unix/Linux. Aquí están los conceptos clave involucrados:

1. **Propietario de archivo (usuario):** Cada archivo y directorio en un sistema Unix/Linux tiene un propietario. El propietario es el usuario que creó o que actualmente posee el archivo o directorio. El propietario tiene ciertos derechos y permisos específicos para ese recurso.

2. **Grupo:** Además del propietario, un archivo o directorio también está asociado a un grupo. Los grupos son colecciones de usuarios y permiten una administración eficiente de permisos. Cada archivo o directorio tiene un grupo principal, y los miembros de ese grupo tienen ciertos permisos en el recurso.

3. **Permisos:** Los archivos y directorios tienen permisos que controlan quién puede realizar acciones específicas, como leer, escribir o ejecutar el recurso. Estos permisos se dividen en tres categorías: propietario, grupo y otros (resto de usuarios).

La modificación de usuario y grupo implica cambiar quién es el propietario y el grupo asociado con un archivo o directorio. Esto se logra principalmente mediante dos comandos:

- **`chown` (change owner):** Este comando se utiliza para cambiar el propietario de un archivo o directorio. La sintaxis básica es:

  ```
  chown nuevo_usuario archivo_o_directorio
  ```

  Por ejemplo, para cambiar el propietario de un archivo llamado "archivo.txt" al usuario "nuevo_usuario":

  ```
  chown nuevo_usuario archivo.txt
  ```

- **`chgrp` (change group):** Este comando se utiliza para cambiar el grupo asociado con un archivo o directorio. La sintaxis básica es:

  ```
  chgrp nuevo_grupo archivo_o_directorio
  ```

  Por ejemplo, para cambiar el grupo de un archivo llamado "archivo.txt" al grupo "nuevo_grupo":

  ```
  chgrp nuevo_grupo archivo.txt
  ```

Estos comandos son útiles para restringir o permitir el acceso a ciertos usuarios y grupos a recursos específicos en el sistema. Por ejemplo, un administrador de sistemas podría cambiar el propietario y el grupo de un directorio que contiene archivos sensibles para garantizar que solo un grupo específico de usuarios tenga acceso a esos archivos.

Es importante tener en cuenta que para utilizar estos comandos, generalmente se requieren privilegios de administrador (superusuario o root), ya que cambiar la propiedad de un archivo o directorio puede tener un impacto significativo en la seguridad del sistema.


## Atributos
Los atributos de acceso en sistemas Unix y Linux controlan los derechos y permisos para archivos y directorios, y son fundamentales para la seguridad y el control de acceso. Estos atributos se asocian con tres categorías de usuarios:

**Propietario (owner)**: El usuario que creó o posee el archivo o directorio.  
**Grupo (group)**: El grupo al que pertenece el archivo o directorio.  
**Otros (others)**: Todos los usuarios que no son propietarios ni pertenecen al grupo asociado.

Existen tres permisos básicos que se pueden otorgar o negar en cada una de estas categorías:  

**Lectura (read, r)**: Permite ver el contenido del archivo o el listado de un directorio.  

**Escritura** (write, w)**: Permite modificar o eliminar el archivo o crear archivos en el directorio.  

**Ejecución (execute, x)**: Para archivos, permite ejecutarlos como programas. Para directorios, permite acceder a su contenido.
El sistema asigna códigos numéricos a estos permisos:  

- Lectura (r): Asigna el valor 4.
- Escritura (w): Asigna el valor 2.
- Ejecución (x): Asigna el valor 1.

Para representar los permisos en forma numérica, se suman estos valores para cada una de las categorías (propietario, grupo y otros). Por ejemplo:

- rwx (Lectura, Escritura y Ejecución) se representa como 7 (4+2+1).
- rw- (Lectura y Escritura, sin Ejecución) se representa como 6 (4+2+0).
- r-x (Lectura y Ejecución, sin Escritura) se representa como 5 (4+0+1).
- r-- (Solo Lectura) se representa como 4 (4+0+0).
Para asignar estos permisos numéricos a un archivo o directorio, se utiliza el comando chmod. Además, para establecer el propietario y el grupo, se utiliza el comando chown.

Ahora, relacionemos esto con la explicación anterior sobre chown:

Cuando usamos chown, estamos cambiando el propietario del archivo o directorio. Por ejemplo, chown nuevo_usuario archivo.txt establece a "nuevo_usuario" como el propietario de "archivo.txt".

Si hablamos de permisos de acceso, podríamos utilizar chmod para asignar permisos específicos al propietario, grupo y otros usuarios. Por ejemplo, si deseamos que el propietario tenga todos los permisos y que el grupo y otros no tengan ningún permiso, podríamos usar chmod 700 archivo.txt. Esto se desglosa como:

- 7 (propietario): rwx (4+2+1)
- 0 (grupo): ---
- 0 (otros): ---
De esta manera, chmod nos permite establecer los permisos numéricos de manera más granular. En el ejemplo, el propietario tiene permisos completos (lectura, escritura y ejecución), mientras que el grupo y otros no tienen ningún permiso.

La combinación de chown y chmod permite a los administradores de sistemas gestionar con precisión quién tiene acceso y qué acciones pueden realizar en archivos y directorios en sistemas Unix y Linux.