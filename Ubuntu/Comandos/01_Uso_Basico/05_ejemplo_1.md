### Ejemplo: Configuración de un Entorno con Permisos Diferenciados

#### Objetivo del Ejemplo

1. Crear un entorno de trabajo para un proyecto llamado `proyecto_empresarial`.
2. Configurar diferentes accesos para tres grupos: `admin_proyecto`, `lectura_proyecto`, y `edicion_proyecto`.
3. Crear archivos dentro del entorno con permisos específicos para cada grupo, simulando escenarios de acceso limitado, lectura compartida, y edición restringida.

#### Paso a Paso

---

### Paso 1: Creación de los Grupos

1. **Crear tres grupos con distintos niveles de acceso**:

   ```bash
   sudo groupadd admin_proyecto
   sudo groupadd lectura_proyecto
   sudo groupadd edicion_proyecto
   ```

   Esto crea los grupos `admin_proyecto`, `lectura_proyecto` y `edicion_proyecto`, que utilizaremos para asignar permisos específicos a los archivos y directorios de nuestro proyecto.

---

### Paso 2: Creación de Usuarios y Asignación a Grupos

2. **Crear tres usuarios y asignarlos a los grupos**:

   ```bash
   sudo adduser usuario_admin
   sudo adduser usuario_lectura
   sudo adduser usuario_edicion
   ```

3. **Agregar los usuarios a sus respectivos grupos**:

   ```bash
   sudo usermod -aG admin_proyecto usuario_admin
   sudo usermod -aG lectura_proyecto usuario_lectura
   sudo usermod -aG edicion_proyecto usuario_edicion
   ```

   Con esto, `usuario_admin` está en el grupo `admin_proyecto`, `usuario_lectura` en `lectura_proyecto`, y `usuario_edicion` en `edicion_proyecto`.

---

### Paso 3: Creación de la Estructura de Directorios

4. **Crear el directorio principal del proyecto**:

   ```bash
   sudo mkdir /proyecto_empresarial
   ```

5. **Crear subdirectorios para cada grupo**:

   ```bash
   sudo mkdir /proyecto_empresarial/admin
   sudo mkdir /proyecto_empresarial/lectura
   sudo mkdir /proyecto_empresarial/edicion
   ```

   Con esto, tenemos una estructura básica de directorios en la que cada subdirectorio representará un área específica para los distintos grupos.

---

### Paso 4: Asignación de Permisos a los Directorios

6. **Asignar permisos específicos para cada subdirectorio**:

   - **Permisos de administrador** (`admin`):

     ```bash
     sudo chown :admin_proyecto /proyecto_empresarial/admin
     sudo chmod 770 /proyecto_empresarial/admin
     ```

     Esto permite que el grupo `admin_proyecto` tenga permisos completos (lectura, escritura y ejecución) en el directorio `admin`. Otros usuarios no tendrán acceso.

   - **Permisos de solo lectura** (`lectura`):

     ```bash
     sudo chown :lectura_proyecto /proyecto_empresarial/lectura
     sudo chmod 750 /proyecto_empresarial/lectura
     ```

     Esto da al grupo `lectura_proyecto` permisos de lectura y ejecución, lo que permite ver el contenido del directorio sin modificarlo.

   - **Permisos de edición restringida** (`edicion`):

     ```bash
     sudo chown :edicion_proyecto /proyecto_empresarial/edicion
     sudo chmod 770 /proyecto_empresarial/edicion
     ```

     Aquí, el grupo `edicion_proyecto` tiene permisos de lectura, escritura y ejecución, mientras que otros usuarios no tienen acceso.

---

### Paso 5: Creación de Archivos con Permisos Específicos

7. **Crear archivos dentro de los directorios y configurar permisos de acceso**:

   - **Archivo de configuración solo accesible para administración**:

     ```bash
     sudo touch /proyecto_empresarial/admin/configuracion.txt
     sudo chmod 600 /proyecto_empresarial/admin/configuracion.txt
     ```

     Este archivo solo será legible y modificable por el propietario, y no será accesible para otros.

   - **Archivo compartido con permisos de solo lectura**:

     ```bash
     sudo touch /proyecto_empresarial/lectura/manual_usuarios.txt
     sudo chmod 440 /proyecto_empresarial/lectura/manual_usuarios.txt
     ```

     El archivo `manual_usuarios.txt` permite al propietario y al grupo `lectura_proyecto` leer el archivo, pero no modificarlo ni ejecutarlo.

   - **Archivo editable para el grupo de edición**:

     ```bash
     sudo touch /proyecto_empresarial/edicion/revision_documento.txt
     sudo chmod 660 /proyecto_empresarial/edicion/revision_documento.txt
     ```

     Aquí, tanto el propietario como el grupo `edicion_proyecto` tienen permisos de lectura y escritura, permitiendo modificaciones. Otros usuarios no tienen acceso.

---

### Paso 6: Modificar Permisos de Archivos con Notación Simbólica

8. **Agregar permisos de ejecución a un archivo específico para el grupo de administración**:

   Supongamos que queremos que el archivo de configuración sea ejecutable por el grupo de administración:

   ```bash
   sudo chmod g+x /proyecto_empresarial/admin/configuracion.txt
   ```

   Esto añade el permiso de ejecución para el grupo `admin_proyecto`, sin cambiar otros permisos.

9. **Eliminar permisos de escritura para otros usuarios en un archivo específico**:

   ```bash
   sudo chmod o-w /proyecto_empresarial/lectura/manual_usuarios.txt
   ```

   Con esto, eliminamos cualquier posibilidad de que usuarios externos puedan escribir en el archivo `manual_usuarios.txt`.

---

### Paso 7: Verificación de Permisos

10. **Comprobar los permisos de los directorios y archivos**:

   Utilizamos el comando `ls -l` para verificar que los permisos estén configurados correctamente.

   ```bash
   ls -l /proyecto_empresarial
   ```

   La salida mostrará algo similar a:

   ```plaintext
   drwxrwx---  2 root admin_proyecto 4096 nov  4 12:00 admin
   drwxr-x---  2 root lectura_proyecto 4096 nov  4 12:00 lectura
   drwxrwx---  2 root edicion_proyecto 4096 nov  4 12:00 edicion
   ```

   También podemos verificar los permisos de los archivos:

   ```bash
   ls -l /proyecto_empresarial/admin/configuracion.txt
   ls -l /proyecto_empresarial/lectura/manual_usuarios.txt
   ls -l /proyecto_empresarial/edicion/revision_documento.txt
   ```

---

### Paso 8: Modificación de Permisos Especiales

11. **Agregar el bit SGID al directorio de edición**:

   Si queremos que todos los archivos creados en `edicion` hereden el grupo `edicion_proyecto`, podemos usar el bit SGID:

   ```bash
   sudo chmod 2770 /proyecto_empresarial/edicion
   ```

   Esto asegura que todos los archivos creados dentro del directorio `edicion` pertenezcan automáticamente al grupo `edicion_proyecto`.

