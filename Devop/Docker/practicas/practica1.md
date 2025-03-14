## Objetivos de la práctica

1. **Instalar y comprobar Docker** en tu máquina (Windows en este caso).
2. **Descargar la imagen oficial de PostgreSQL** desde Docker Hub.
3. **Ejecutar el contenedor** de PostgreSQL con:
   - Un directorio externo en tu disco local para guardar los datos (persistencia).
   - Configuración de usuario, contraseña y base de datos inicial.
   - Puertos abiertos para que puedas conectarte con pgAdmin o cualquier otra herramienta de gestión de bases de datos.

4. **Probar la conexión** a PostgreSQL con una herramienta externa (por ejemplo, pgAdmin).



## Pasos detallados

### 1. Verificar que Docker está instalado y funcionando
- Abre una ventana de **PowerShell** o **CMD**.
- Ejecuta el siguiente comando para comprobar la versión de Docker:

  ```bash
  docker version
  ```

  Si Docker está instalado correctamente, verás información sobre la versión de Docker instalada.  

### 2. Crear un directorio para la persistencia de datos
Crea un directorio en la misma carpeta donde vas a trabajar, por ejemplo:

```bash
mkdir postgres-data
```

Este directorio será montado como volumen en el contenedor para que los datos de la base de datos se conserven fuera del contenedor.

### 3. Elegir los parámetros de PostgreSQL
Define las variables que utilizarás para la configuración de tu base de datos:
- `POSTGRES_USER`  
- `POSTGRES_PASSWORD`  
- `POSTGRES_DB`  

Por ejemplo:
- Usuario: `usuario_prueba`
- Contraseña: `pass_prueba`
- Base de datos: `db_prueba`

### 4. Descargar y ejecutar la imagen oficial de PostgreSQL
Ahora utiliza el comando `docker run` para lanzar el contenedor:

```bash
docker run -d ^
  --name mi_postgres ^
  -p 5432:5432 ^
  -e POSTGRES_USER=usuario_prueba ^
  -e POSTGRES_PASSWORD=pass_prueba ^
  -e POSTGRES_DB=db_prueba ^
  -v "%cd%/postgres-data:/var/lib/postgresql/data" ^
  postgres:latest
```

```powershell
docker run -d `
  --name mi_postgres `
  -p 5432:5432 `
  -e POSTGRES_USER=usuario_prueba `
  -e POSTGRES_PASSWORD=pass_prueba `
  -e POSTGRES_DB=db_prueba `
  -v "${PWD}/postgres-data:/var/lib/postgresql/data" `
  postgres:latest
```

**Explicación de cada parte del comando**:

- `-d`: Ejecuta el contenedor en segundo plano (detached).
- `--name mi_postgres`: Asigna el nombre `mi_postgres` al contenedor para identificarlo fácilmente.
- `-p 5432:5432`: Hace que el puerto 5432 del contenedor se publique en el puerto 5432 de tu máquina.  
- `-e POSTGRES_USER=usuario_prueba`: Variable de entorno para configurar el usuario de la base de datos.  
- `-e POSTGRES_PASSWORD=pass_prueba`: Variable de entorno para la contraseña.  
- `-e POSTGRES_DB=db_prueba`: Variable de entorno para que se cree automáticamente una base de datos con este nombre.  
- `-v "%cd%/postgres-data:/var/lib/postgresql/data"`: Monta la carpeta local `postgres-data` como volumen en la ruta interna `/var/lib/postgresql/data`. Se usa `"%cd%"` para indicar el directorio actual en Windows.
- `postgres:latest`: Indica que usas la imagen oficial de PostgreSQL en su versión más reciente.

**Nota**: En Windows, la sintaxis del salto de línea con `^` funciona en **CMD** y en **PowerShell** puedes hacer algo similar. Si prefieres simplificar, puedes poner todo el comando en una sola línea.

### 5. Verificar que el contenedor está en ejecución

Para asegurarte de que el contenedor se está ejecutando correctamente, utiliza:

```bash
docker ps
```

Verás una tabla con tu contenedor `mi_postgres` y el estado `Up` si todo está correcto.

### 6. Conexión a PostgreSQL (pgAdmin u otra herramienta)
- Abre **pgAdmin** (o la herramienta que prefieras).
- Agrega un nuevo servidor (Add New Server) y especifica:
  - **Name**: Un nombre cualquiera para identificar tu servidor (por ejemplo, `MiServidorPostgres`).
  - **Host**: `localhost` (o la IP de tu máquina, si conectas desde otra computadora).
  - **Port**: `5432`.
  - **Username**: `usuario_prueba` (el valor de `POSTGRES_USER` que configuraste).
  - **Password**: `pass_prueba` (el valor de `POSTGRES_PASSWORD` que configuraste).
  - **Database**: opcionalmente `db_prueba` si quieres conectarte directamente a esa base de datos.

Si todo está correcto, podrás conectarte y ver la base de datos `db_prueba` dentro de pgAdmin.



## Limpieza y buenas prácticas
- Cuando termines de practicar, puedes detener el contenedor sin perder los datos:

  ```bash
  docker stop mi_postgres
  ```

- Y si quieres borrarlo completamente (ojo, no borra la carpeta local `postgres-data`):

  ```bash
  docker rm mi_postgres
  ```

- Si deseas **reiniciar** el contenedor en otro momento, el comando `docker run` anterior creará uno nuevo, que volverá a usar los datos en la carpeta `postgres-data`.


