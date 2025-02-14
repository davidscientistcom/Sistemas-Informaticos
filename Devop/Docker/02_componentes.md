# Introducción a Docker: Imágenes y Contenedores

Docker se ha consolidado como una herramienta fundamental en el desarrollo, despliegue y operación de aplicaciones modernas. Permite empaquetar aplicaciones y sus dependencias en unidades aisladas denominadas **contenedores**, garantizando que se ejecuten de la misma manera en cualquier entorno. En este documento, explicaremos detalladamente qué son las imágenes y los contenedores en Docker, cómo gestionarlos mediante comandos (presentados en tablas), y cómo interactuar con ellos (leer logs y acceder a su entorno de ejecución). Además, introduciremos el concepto de **Dockerfile**, que es la piedra angular para la automatización de la construcción de imágenes, sin entrar en la creación de uno en este momento.


## 1. Imágenes en Docker

### 1.1. ¿Qué es una imagen?

Una **imagen** en Docker es una plantilla inmutable que contiene todo lo necesario para ejecutar una aplicación: el sistema de archivos, bibliotecas, dependencias y configuraciones. Las imágenes se basan en capas, lo que permite reutilizar partes comunes y optimizar el almacenamiento y la transferencia. Por ejemplo, una imagen de Ubuntu incluirá el sistema operativo base, y sobre ella se pueden añadir aplicaciones o configuraciones específicas.

### 1.2. Creación y gestión de imágenes

- **Construcción:** Las imágenes se pueden construir a partir de un archivo llamado **Dockerfile** (más adelante se explica en detalle) o descargarlas de repositorios como Docker Hub.
- **Distribución:** Una vez construida, una imagen se puede compartir, versionar y desplegar en distintos entornos.
- **Inmutabilidad:** Cada vez que se crea una nueva imagen, se genera una versión inmutable. Esto garantiza que el entorno de ejecución siempre sea el mismo, independientemente de dónde se despliegue.

### 1.3. Tabla de comandos para la gestión de imágenes

| **Comando**                           | **Descripción**                                                         | **Ejemplo**                                |
|---------------------------------------|-------------------------------------------------------------------------|--------------------------------------------|
| `docker images`                       | Lista todas las imágenes disponibles localmente.                      | `docker images`                            |
| `docker build`                        | Construye una imagen a partir de un Dockerfile.                         | `docker build -t mi_imagen .`              |
| `docker pull`                         | Descarga una imagen desde un repositorio (por ejemplo, Docker Hub).     | `docker pull ubuntu`                       |
| `docker rmi`                          | Elimina una o más imágenes de la máquina local.                         | `docker rmi mi_imagen`                       |
| `docker tag`                          | Asigna una nueva etiqueta a una imagen.                                 | `docker tag imagen_id nombre:tag`          |



## 2. Contenedores en Docker

### 2.1. ¿Qué es un contenedor?

Un **contenedor** es una instancia en ejecución de una imagen. Mientras que la imagen es la plantilla inmutable, el contenedor representa el entorno activo donde la aplicación se ejecuta. Cada contenedor se aísla del sistema host, pero puede compartir el núcleo del sistema operativo, lo que lo hace muy ligero y eficiente.

### 2.2. Diferencias clave entre imágenes y contenedores

- **Imagen:** Plantilla estática, inmutable, que define el entorno de ejecución.
- **Contenedor:** Instancia en ejecución de una imagen, que puede tener estados cambiantes, datos volátiles y puede ser iniciado, detenido o eliminado según sea necesario.

### 2.3. Gestión de contenedores: comandos y ejemplos

| **Comando**                           | **Descripción**                                                         | **Ejemplo**                                           |
|---------------------------------------|-------------------------------------------------------------------------|-------------------------------------------------------|
| `docker run`                          | Crea y ejecuta un contenedor a partir de una imagen.                    | `docker run -d --name mi_contenedor ubuntu`           |
| `docker ps`                           | Lista los contenedores en ejecución (con `-a` se listan también los detenidos). | `docker ps` o `docker ps -a`                           |
| `docker stop`                         | Detiene un contenedor en ejecución.                                     | `docker stop mi_contenedor`                           |
| `docker rm`                           | Elimina un contenedor detenido.                                          | `docker rm mi_contenedor`                             |
| `docker exec`                         | Ejecuta un comando en un contenedor en ejecución.                      | `docker exec -it mi_contenedor bash`                  |
| `docker logs`                         | Muestra los logs (registros) de un contenedor.                         | `docker logs mi_contenedor`                           |



## 3. Gestión de Logs y Acceso a Contenedores

### 3.1. Lectura de logs

Los logs en Docker permiten monitorear y depurar la ejecución de los contenedores. Con el comando `docker logs` se pueden visualizar los mensajes que la aplicación genera en su salida estándar (stdout) y error estándar (stderr).

**Ejemplo:**
```bash
docker logs mi_contenedor
```
Además, se pueden utilizar opciones como:
- `--tail N`: Muestra solo las últimas N líneas.
- `-f` o `--follow`: Permite seguir en tiempo real la salida de logs.

**Ejemplo combinado:**
```bash
docker logs -f --tail 20 mi_contenedor
```

### 3.2. Acceso al interior de un contenedor

Para interactuar directamente con el entorno interno de un contenedor (por ejemplo, para depuración o inspección), se utiliza el comando `docker exec`. Con la opción `-it` se abre una terminal interactiva.

**Ejemplo:**
```bash
docker exec -it mi_contenedor bash
```
En este ejemplo, se abre una shell de Bash dentro del contenedor, permitiendo ejecutar comandos de forma interactiva. Si el contenedor no cuenta con Bash instalado, se puede utilizar `sh` u otro intérprete disponible.


## 4. Introducción al Dockerfile

### 4.1. ¿Qué es un Dockerfile?

Un **Dockerfile** es un archivo de texto que contiene una serie de instrucciones que Docker utiliza para construir una imagen de forma automatizada. Cada instrucción en el Dockerfile genera una capa en la imagen, lo que permite la reutilización y el cacheo de pasos ya realizados en construcciones anteriores.

### 4.2. Componentes básicos de un Dockerfile

Aunque en este documento no crearemos un Dockerfile, es importante conocer sus componentes fundamentales:

- **FROM:** Especifica la imagen base a partir de la cual se construirá la nueva imagen.
- **RUN:** Ejecuta comandos durante el proceso de construcción (por ejemplo, para instalar paquetes).
- **CMD:** Define el comando predeterminado que se ejecutará al iniciar el contenedor.
- **COPY/ADD:** Copian archivos desde el sistema host hacia el sistema de archivos del contenedor.
- **EXPOSE:** Indica los puertos en los que la aplicación escucha.

**Ejemplo conceptual (sin ejecutarlo):**
```dockerfile
# Utiliza una imagen base de Ubuntu
FROM ubuntu:20.04

# Actualiza el sistema e instala dependencias
RUN apt-get update && apt-get install -y \
    curl \
    git

# Copia la aplicación al contenedor
COPY . /app

# Define el directorio de trabajo
WORKDIR /app

# Expone el puerto 80
EXPOSE 80

# Comando por defecto al iniciar el contenedor
CMD ["./iniciar.sh"]
```
Este ejemplo muestra la estructura básica de un Dockerfile y cómo se pueden encadenar instrucciones para preparar un entorno de ejecución.


## 5. Conclusiones y Buenas Prácticas

Hemos visto que en Docker:
- **Las imágenes** son plantillas inmutables que contienen el sistema operativo, aplicaciones y dependencias.
- **Los contenedores** son instancias en ejecución de estas imágenes, ofreciendo un entorno aislado y reproducible.
- Se dispone de una serie de comandos que facilitan la gestión tanto de imágenes como de contenedores, permitiendo listar, construir, ejecutar, detener, acceder y eliminar de forma sencilla.
- Es esencial monitorear los logs de los contenedores para la depuración y mantenimiento, utilizando `docker logs`.
- El comando `docker exec` permite acceder al interior de un contenedor, facilitando tareas de inspección y diagnóstico.
- Un **Dockerfile** es la herramienta que automatiza la construcción de imágenes, integrando todas las instrucciones necesarias para preparar el entorno de la aplicación.

### Recomendaciones adicionales:
- **Versiona tus imágenes:** Utiliza etiquetas (tags) para mantener un control de versiones.
- **Mantén Dockerfiles limpios y comentados:** Esto facilitará el mantenimiento y la comprensión por parte de otros miembros del equipo.
- **Utiliza volúmenes:** Para persistir datos y facilitar la depuración, especialmente en entornos de desarrollo.

