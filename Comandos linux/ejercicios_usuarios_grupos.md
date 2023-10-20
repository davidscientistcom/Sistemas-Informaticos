
## Ejercicio 1: Crear un usuario

Objetivo: Crear un nuevo usuario en el sistema.

1. Abre una terminal en tu sistema Unix/Linux.

2. Utiliza el comando `useradd` para crear un nuevo usuario llamado "nuevo_usuario".

   ```shell
   sudo useradd nuevo_usuario
   ```

3. Establece una contraseña para el nuevo usuario utilizando el comando `passwd`.

   ```shell
   sudo passwd nuevo_usuario
   ```

4. Ingresa una contraseña segura cuando se te solicite y confírmala.

## Ejercicio 2: Crear un grupo

Objetivo: Crear un nuevo grupo en el sistema.

1. Abre una terminal en tu sistema Unix/Linux.

2. Utiliza el comando `groupadd` para crear un nuevo grupo llamado "nuevo_grupo".

   ```shell
   sudo groupadd nuevo_grupo
   ```

3. Verifica que el grupo se haya creado correctamente ejecutando el comando `getent group nuevo_grupo`.

Estos ejercicios permitirán a los alumnos practicar la creación de usuarios y grupos en un sistema Unix/Linux. Esto es fundamental para la administración de usuarios y permisos en el sistema.