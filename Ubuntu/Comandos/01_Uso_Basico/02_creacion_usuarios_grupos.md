### Creación y Gestión de Usuarios y Grupos en Ubuntu Server

La gestión de usuarios y grupos en un sistema operativo basado en Linux como Ubuntu es esencial para administrar los permisos y el acceso a recursos. En este documento, detallaremos cómo crear usuarios y asignarlos a grupos. También abordaremos la creación de una estructura de directorios y archivos donde cada grupo tenga distintos niveles de acceso y permisos.

#### 1. Creación de Usuarios en Ubuntu

Para crear usuarios, utilizamos el comando `useradd`, que nos permite añadir nuevos usuarios al sistema con configuraciones básicas. Sin embargo, el comando `adduser` suele ser preferido en Ubuntu, ya que es más intuitivo y realiza algunas configuraciones adicionales automáticamente.

##### Comandos Básicos para la Creación de Usuarios

1. **Crear un usuario**:

   ```bash
   sudo adduser nombre_usuario
   ```

   Este comando crea el usuario `nombre_usuario`, solicita una contraseña y establece un directorio home en `/home/nombre_usuario`. También le asigna un ID de usuario (UID) y un grupo privado con el mismo nombre que el usuario.

2. **Eliminar un usuario**:

   ```bash
   sudo deluser nombre_usuario
   ```

   Este comando elimina el usuario, pero deja su directorio home a menos que se especifique lo contrario. Para eliminar el directorio del usuario:

   ```bash
   sudo deluser --remove-home nombre_usuario
   ```

#### 2. Tipos de Usuarios

Existen varios tipos de usuarios en un sistema Linux:

- **Usuario root**: El administrador del sistema, con todos los permisos.
- **Usuarios estándar**: Usuarios normales que tienen acceso limitado al sistema y a los recursos.
- **Usuarios del sistema**: Usuarios creados automáticamente por servicios y aplicaciones (como `www-data` para el servidor web). No suelen iniciar sesión y sirven para ejecutar procesos específicos.

### 3. Creación y Gestión de Grupos

Los grupos en Linux permiten organizar usuarios para compartir permisos y acceder a recursos de manera controlada. 

1. **Crear un grupo**:

   ```bash
   sudo groupadd nombre_grupo
   ```

   Este comando crea un grupo llamado `nombre_grupo`. Los grupos suelen emplearse para controlar los permisos sobre archivos y directorios, lo cual permite organizar de forma eficiente los accesos.

2. **Eliminar un grupo**:

   ```bash
   sudo groupdel nombre_grupo
   ```

   Este comando elimina el grupo `nombre_grupo`. Asegúrate de que no haya usuarios asociados al grupo antes de eliminarlo.

#### 4. Asignación de Usuarios a Grupos

La asignación de usuarios a grupos permite definir quién tiene acceso a qué recursos. Es posible asignar múltiples grupos a un usuario, y un grupo puede contener varios usuarios.

1. **Agregar un usuario a un grupo**:

   ```bash
   sudo usermod -aG nombre_grupo nombre_usuario
   ```

   Este comando agrega el usuario `nombre_usuario` al grupo `nombre_grupo`. Es importante el uso de `-a` para añadir el usuario sin quitarlo de otros grupos a los que ya pertenezca.

2. **Ver los grupos a los que pertenece un usuario**:

   ```bash
   groups nombre_usuario
   ```

   Esto muestra todos los grupos a los que el usuario pertenece actualmente.

3. **Remover un usuario de un grupo**:

   ```bash
   sudo gpasswd -d nombre_usuario nombre_grupo
   ```

   Este comando elimina el usuario `nombre_usuario` del grupo `nombre_grupo`.

### 5. Estructura de Directorios con Permisos por Grupos

Vamos a crear una estructura de directorios con varios niveles de permisos. Tendremos dos grupos de usuarios con distintos permisos sobre los directorios y archivos.

#### Paso a Paso

1. **Crear grupos para los usuarios**:

   ```bash
   sudo groupadd grupo_lectura
   sudo groupadd grupo_escritura
   ```

   Creamos dos grupos: `grupo_lectura` y `grupo_escritura`.

2. **Crear usuarios y asignarlos a los grupos**:

   ```bash
   sudo adduser usuario1
   sudo adduser usuario2
   sudo adduser usuario3
   sudo usermod -aG grupo_lectura usuario1
   sudo usermod -aG grupo_escritura usuario2
   sudo usermod -aG grupo_lectura usuario3
   ```

   En este caso, hemos creado tres usuarios. `usuario1` y `usuario3` están en el grupo `grupo_lectura`, mientras que `usuario2` está en el grupo `grupo_escritura`.

3. **Crear directorios y asignar permisos**:

   ```bash
   sudo mkdir /directorio_grupo_lectura
   sudo mkdir /directorio_grupo_escritura
   ```

   Creamos dos directorios: `directorio_grupo_lectura` y `directorio_grupo_escritura`.

4. **Asignar grupos y permisos a los directorios**:

   - Directorio de solo lectura para `grupo_lectura`:

     ```bash
     sudo chown :grupo_lectura /directorio_grupo_lectura
     sudo chmod 750 /directorio_grupo_lectura
     ```

     Con `chmod 750`, estamos configurando permisos para que:
     - El propietario (root) tenga permisos completos (lectura, escritura, ejecución).
     - Los miembros del grupo `grupo_lectura` tengan permisos de lectura y ejecución.
     - Los demás usuarios no tengan acceso.

   - Directorio de lectura y escritura para `grupo_escritura`:

     ```bash
     sudo chown :grupo_escritura /directorio_grupo_escritura
     sudo chmod 770 /directorio_grupo_escritura
     ```

     Con `chmod 770`, los miembros del grupo `grupo_escritura` tienen permisos de lectura, escritura y ejecución, mientras que los demás no tienen acceso.

5. **Comprobar los permisos**:

   Puedes verificar los permisos de los directorios con el siguiente comando:

   ```bash
   ls -ld /directorio_grupo_lectura /directorio_grupo_escritura
   ```

   Esto mostrará una salida similar a:

   ```bash
   drwxr-x--- 2 root grupo_lectura 4096 nov  4 10:00 /directorio_grupo_lectura
   drwxrwx--- 2 root grupo_escritura 4096 nov  4 10:00 /directorio_grupo_escritura
   ```

#### 6. Prueba de Acceso y Resultados

1. **Acceso al directorio**: 
   - Los usuarios en `grupo_lectura` pueden acceder a `/directorio_grupo_lectura` para leer, pero no pueden modificar nada.
   - Los usuarios en `grupo_escritura` pueden acceder y modificar archivos en `/directorio_grupo_escritura`.

2. **Creación de archivos en el directorio**:
   - Los usuarios de `grupo_escritura` pueden crear archivos dentro de `/directorio_grupo_escritura` usando comandos como `touch nombre_archivo`.
