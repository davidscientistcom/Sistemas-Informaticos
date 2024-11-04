### Ejemplo Completo: Configuración de Usuarios, Grupos y Permisos en un Proyecto

#### Objetivo

Vamos a simular un proyecto llamado `proyecto_empresa` que involucra tres roles de usuario: **administrador**, **editor** y **lector**. A través de este ejemplo:

1. Crearemos usuarios y los asignaremos a roles específicos mediante grupos.
2. Configuraremos una estructura de directorios donde cada grupo tenga acceso diferenciado a recursos.
3. Aplicaremos permisos específicos para cada archivo y directorio.

#### Paso 1: Crear Grupos

Comenzamos creando tres grupos que representan los diferentes roles de acceso dentro del proyecto.

```bash
sudo groupadd admin_proyecto      # Grupo para los administradores del proyecto
sudo groupadd lectura_proyecto    # Grupo para usuarios con permisos de solo lectura
sudo groupadd edicion_proyecto    # Grupo para usuarios que pueden editar contenido
```

---

#### Paso 2: Crear Usuarios y Asignarlos a los Grupos

Creamos usuarios y los asignamos a cada grupo según el rol que desempeñarán en el proyecto.

```bash
# Crear usuario de administración y asignarlo al grupo de administradores
sudo adduser usuario_admin
sudo usermod -aG admin_proyecto usuario_admin

# Crear usuario de solo lectura y asignarlo al grupo de lectura
sudo adduser usuario_lectura
sudo usermod -aG lectura_proyecto usuario_lectura

# Crear usuario de edición y asignarlo al grupo de editores
sudo adduser usuario_edicion
sudo usermod -aG edicion_proyecto usuario_edicion
```

Cada usuario ahora forma parte del grupo correspondiente y tendrá permisos específicos en función de su rol.

---

#### Paso 3: Crear la Estructura de Directorios del Proyecto

Creamos el directorio principal del proyecto y subdirectorios que serán utilizados por los diferentes roles.

```bash
# Crear el directorio principal del proyecto
sudo mkdir /proyecto_empresa

# Crear subdirectorios para cada rol
sudo mkdir /proyecto_empresa/admin      # Directorio para administradores
sudo mkdir /proyecto_empresa/lectura    # Directorio para acceso de solo lectura
sudo mkdir /proyecto_empresa/edicion    # Directorio para los editores
```

---

#### Paso 4: Configurar Permisos de los Directorios

Ahora, vamos a asignar permisos específicos para cada directorio de modo que solo el grupo designado tenga acceso a los archivos y carpetas correspondientes.

1. **Permisos para el Directorio de Administración**:

   ```bash
   sudo chown :admin_proyecto /proyecto_empresa/admin
   sudo chmod 770 /proyecto_empresa/admin
   ```

   Esto permite que el grupo `admin_proyecto` tenga permisos completos (lectura, escritura y ejecución) en el directorio `admin`. Los demás usuarios no tienen acceso.

2. **Permisos para el Directorio de Solo Lectura**:

   ```bash
   sudo chown :lectura_proyecto /proyecto_empresa/lectura
   sudo chmod 750 /proyecto_empresa/lectura
   ```

   Con estos permisos, el grupo `lectura_proyecto` puede acceder al directorio `lectura` para ver el contenido, pero no puede modificarlo. Otros usuarios no tienen acceso.

3. **Permisos para el Directorio de Edición**:

   ```bash
   sudo chown :edicion_proyecto /proyecto_empresa/edicion
   sudo chmod 770 /proyecto_empresa/edicion
   ```

   Esto permite que el grupo `edicion_proyecto` tenga permisos completos en el directorio `edicion`, mientras que otros usuarios no tienen acceso.

---

#### Paso 5: Creación de Archivos y Asignación de Permisos Específicos

Dentro de cada directorio, vamos a crear archivos y asignar permisos específicos para controlar el acceso.

1. **Archivo de Configuración para Administradores**:

   Este archivo solo debe ser accesible y editable por los administradores.

   ```bash
   sudo touch /proyecto_empresa/admin/configuracion_admin.txt
   sudo chown usuario_admin:admin_proyecto /proyecto_empresa/admin/configuracion_admin.txt
   sudo chmod 600 /proyecto_empresa/admin/configuracion_admin.txt
   ```

   Con `chmod 600`, solo el propietario (usuario_admin) tiene permisos de lectura y escritura; el grupo y otros usuarios no tienen acceso.

2. **Archivo de Documentación de Solo Lectura**:

   En el directorio de `lectura`, crearemos un archivo accesible solo para lectura, que los editores y administradores también pueden ver pero no modificar.

   ```bash
   sudo touch /proyecto_empresa/lectura/manual_usuarios.txt
   sudo chown root:lectura_proyecto /proyecto_empresa/lectura/manual_usuarios.txt
   sudo chmod 440 /proyecto_empresa/lectura/manual_usuarios.txt
   ```

   Con `chmod 440`, tanto el propietario como el grupo tienen permiso de lectura sin posibilidad de modificación.

3. **Documento Editable para el Grupo de Edición**:

   En el directorio `edicion`, crearemos un archivo que pueda ser editado solo por los editores.

   ```bash
   sudo touch /proyecto_empresa/edicion/revision_proyecto.txt
   sudo chown usuario_edicion:edicion_proyecto /proyecto_empresa/edicion/revision_proyecto.txt
   sudo chmod 660 /proyecto_empresa/edicion/revision_proyecto.txt
   ```

   Con `chmod 660`, el propietario y el grupo tienen permisos de lectura y escritura, mientras que otros usuarios no tienen acceso.

---

#### Paso 6: Configuración Adicional con Notación Simbólica

Algunos ajustes de permisos específicos pueden hacerse usando la notación simbólica para modificar permisos de una manera intuitiva.

1. **Agregar permisos de ejecución para el grupo de administradores en el archivo de configuración**:

   ```bash
   sudo chmod g+x /proyecto_empresa/admin/configuracion_admin.txt
   ```

   Esto permite que los administradores puedan ejecutar el archivo de configuración si fuese necesario (por ejemplo, en el caso de que sea un script).

2. **Eliminar permisos de escritura para otros usuarios en el archivo de solo lectura**:

   ```bash
   sudo chmod o-w /proyecto_empresa/lectura/manual_usuarios.txt
   ```

   Esto asegura que no haya permisos de escritura para otros usuarios en el archivo `manual_usuarios.txt`.

---

#### Paso 7: Verificación de los Permisos

Para confirmar que todos los permisos se han aplicado correctamente, podemos listar el contenido del directorio principal junto con los permisos asignados:

```bash
ls -l /proyecto_empresa
```

Este comando debería mostrar los permisos configurados para cada directorio y archivo, algo similar a:

```plaintext
drwxrwx---  2 root admin_proyecto     4096 nov  4 14:00 admin
drwxr-x---  2 root lectura_proyecto   4096 nov  4 14:00 lectura
drwxrwx---  2 root edicion_proyecto   4096 nov  4 14:00 edicion
```

También podemos verificar cada archivo individualmente para asegurarnos de que los permisos son correctos:

```bash
ls -l /proyecto_empresa/admin/configuracion_admin.txt
ls -l /proyecto_empresa/lectura/manual_usuarios.txt
ls -l /proyecto_empresa/edicion/revision_proyecto.txt
```

---
