## Crear grupos y usuarios

Crear usuarios y grupos en sistemas Unix y Linux es una parte fundamental de la administración del sistema. Los usuarios son las cuentas individuales que utilizan las personas para acceder al sistema, mientras que los grupos son colecciones de usuarios que se utilizan para administrar permisos y facilitar la gestión de recursos compartidos. A continuación, te explicaré cómo crear usuarios y grupos y proporcionaré ejemplos para que los alumnos puedan practicar.

**Crear un usuario:**

Para crear un nuevo usuario en un sistema Unix/Linux, se utiliza el comando `useradd`. Aquí está el formato básico:

```shell
sudo useradd nombre_de_usuario
```

- `nombre_de_usuario` es el nombre de usuario que deseas crear.

Por ejemplo, para crear un usuario llamado "nuevo_usuario":

```shell
sudo useradd nuevo_usuario
```

Este comando crea el usuario, pero no establece una contraseña inicial. Para establecer una contraseña, puedes usar el comando `passwd`:

```shell
sudo passwd nuevo_usuario
```

El sistema te pedirá que ingreses la nueva contraseña y la confirmes.

**Crear un grupo:**

Para crear un nuevo grupo, se utiliza el comando `groupadd`. Aquí está el formato básico:

```shell
sudo groupadd nombre_del_grupo
```

- `nombre_del_grupo` es el nombre del grupo que deseas crear.

Por ejemplo, para crear un grupo llamado "nuevo_grupo":

```shell
sudo groupadd nuevo_grupo
```
