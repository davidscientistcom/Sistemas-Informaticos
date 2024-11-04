### Ejercicio 1: Crear un directorio y asignar permisos de lectura y ejecución a un grupo específico

1. **Instrucciones**: Crea un directorio llamado `docs_proyectos` y asigna permisos de lectura y ejecución para el grupo `grupo_proyectos`.
2. **Resolución**:

   ```bash
   sudo mkdir /docs_proyectos
   sudo groupadd grupo_proyectos
   sudo chown :grupo_proyectos /docs_proyectos
   sudo chmod 750 /docs_proyectos
   ```

3. **Explicación**: Este comando asigna al grupo `grupo_proyectos` permisos de lectura y ejecución sobre el directorio, mientras que el propietario (root) tiene permisos completos. Otros usuarios no pueden acceder.

### Ejercicio 2: Cambiar los permisos de un archivo para permitir solo la lectura

1. **Instrucciones**: Crea un archivo llamado `confidencial.txt` dentro del directorio `docs_proyectos` y cambia sus permisos para que solo sea legible.
2. **Resolución**:

   ```bash
   sudo touch /docs_proyectos/confidencial.txt
   sudo chmod 440 /docs_proyectos/confidencial.txt
   ```

3. **Explicación**: `chmod 440` hace que solo el propietario y el grupo tengan permisos de lectura sobre el archivo, mientras que otros usuarios no tienen acceso.

### Ejercicio 3: Crear un grupo y asignar varios usuarios al grupo

1. **Instrucciones**: Crea un grupo llamado `grupo_ti` y agrega los usuarios `tecnico1`, `tecnico2` y `tecnico3` a este grupo.
2. **Resolución**:

   ```bash
   sudo groupadd grupo_ti
   sudo usermod -aG grupo_ti tecnico1
   sudo usermod -aG grupo_ti tecnico2
   sudo usermod -aG grupo_ti tecnico3
   ```

3. **Explicación**: Cada comando `usermod` agrega un usuario al grupo `grupo_ti` sin quitarlo de otros grupos.

### Ejercicio 4: Crear un archivo y asignar permisos de lectura y escritura solo para el propietario

1. **Instrucciones**: Crea un archivo llamado `notas_personales.txt` y cambia sus permisos para que solo el propietario tenga permisos de lectura y escritura.
2. **Resolución**:

   ```bash
   sudo touch notas_personales.txt
   sudo chmod 600 notas_personales.txt
   ```

3. **Explicación**: `chmod 600` permite que solo el propietario lea y modifique el archivo, bloqueando el acceso al grupo y a otros usuarios.

### Ejercicio 5: Crear un directorio con permisos de solo ejecución para el grupo

1. **Instrucciones**: Crea un directorio llamado `scripts_ejecutables` y configura permisos de solo ejecución para el grupo `grupo_usuarios`.
2. **Resolución**:

   ```bash
   sudo mkdir /scripts_ejecutables
   sudo groupadd grupo_usuarios
   sudo chown :grupo_usuarios /scripts_ejecutables
   sudo chmod 710 /scripts_ejecutables
   ```

3. **Explicación**: El grupo `grupo_usuarios` tiene permisos de ejecución, lo que permite acceder al directorio, pero no leer ni modificar su contenido.

### Ejercicio 6: Cambiar el propietario de un archivo

1. **Instrucciones**: Cambia el propietario del archivo `confidencial.txt` creado en el ejercicio 2 para que `usuario1` sea el nuevo propietario.
2. **Resolución**:

   ```bash
   sudo chown usuario1 /docs_proyectos/confidencial.txt
   ```

3. **Explicación**: `chown` cambia el propietario del archivo a `usuario1`, mientras mantiene el grupo y los permisos establecidos.

### Ejercicio 7: Asignar permisos de escritura solo al grupo

1. **Instrucciones**: Cambia los permisos del archivo `confidencial.txt` para que el grupo tenga permisos de escritura, mientras que el propietario solo puede leer.
2. **Resolución**:

   ```bash
   sudo chmod 460 /docs_proyectos/confidencial.txt
   ```

3. **Explicación**: `chmod 460` permite que el propietario solo lea y el grupo solo escriba, mientras que otros usuarios no tienen acceso.

### Ejercicio 8: Listar todos los usuarios de un grupo

1. **Instrucciones**: Lista todos los usuarios que pertenecen al grupo `grupo_ti` creado en el ejercicio 3.
2. **Resolución**:

   ```bash
   getent group grupo_ti
   ```

3. **Explicación**: Este comando muestra el grupo `grupo_ti` junto con los usuarios asignados, lo que permite verificar los miembros actuales del grupo.

### Ejercicio 9: Crear un directorio compartido con permisos especiales (SGID)

1. **Instrucciones**: Crea un directorio llamado `proyectos_compartidos` y configura el bit SGID para que todos los archivos creados dentro tengan el grupo `grupo_proyectos`.
2. **Resolución**:

   ```bash
   sudo mkdir /proyectos_compartidos
   sudo chown :grupo_proyectos /proyectos_compartidos
   sudo chmod 2770 /proyectos_compartidos
   ```

3. **Explicación**: El bit SGID (`chmod 2770`) asegura que todos los archivos creados dentro del directorio hereden el grupo `grupo_proyectos`, facilitando el trabajo colaborativo en equipo.

### Ejercicio 10: Denegar permisos de ejecución a otros usuarios en un archivo específico

1. **Instrucciones**: Crea un archivo ejecutable `script_secreto.sh` y configura los permisos para que solo el propietario y el grupo puedan ejecutarlo.
2. **Resolución**:

   ```bash
   sudo touch script_secreto.sh
   sudo chmod 750 script_secreto.sh
   ```

3. **Explicación**: `chmod 750` permite al propietario ejecutar el archivo, al grupo leer y ejecutar, y deniega todos los permisos a otros usuarios.
