## 1. Introducción

Un Dockerfile es un archivo de texto que contiene una serie de instrucciones que Docker interpreta para construir una imagen. Cada instrucción genera una **capa** en la imagen, lo que permite aprovechar el *cacheo* y reutilizar partes comunes en múltiples construcciones. Esto no solo optimiza el proceso de construcción, sino que también hace que la imagen resultante sea predecible y reproducible en cualquier entorno.

El proceso de construcción se realiza con el comando:

```bash
docker build -t nombre_imagen .
```

Este comando lee el Dockerfile ubicado en el directorio actual y crea una imagen etiquetada como `nombre_imagen`.



## 2. Estructura y Sintaxis de un Dockerfile

Un Dockerfile está compuesto por líneas de instrucciones y comentarios. Cada línea (o grupo de líneas continuadas) representa una instrucción que Docker ejecutará durante la construcción de la imagen.

- **Comentarios:** Se inician con el símbolo `#` y no se ejecutan.
  
  ```dockerfile
  # Este es un comentario
  ```

- **Instrucciones:** Se escriben en mayúsculas (por convención) y deben seguir un orden lógico para definir correctamente la imagen.

La estructura básica de un Dockerfile suele incluir:

1. **FROM:** Define la imagen base.
2. **LABEL:** Permite añadir metadatos a la imagen.
3. **ARG y ENV:** Definen variables que se usan durante la construcción y en tiempo de ejecución, respectivamente.
4. **RUN:** Ejecuta comandos en la fase de construcción.
5. **COPY/ADD:** Copian archivos desde el sistema host hacia la imagen.
6. **WORKDIR:** Establece el directorio de trabajo.
7. **EXPOSE:** Informa sobre los puertos que se usarán.
8. **CMD y ENTRYPOINT:** Especifican el comando que se ejecutará al iniciar el contenedor.
9. **Otros:** Instrucciones como `USER`, `VOLUME` o `HEALTHCHECK` que configuran aspectos adicionales.



## 3. Instrucciones Fundamentales y Opciones

A continuación, se describen las instrucciones más comunes, junto con sus opciones y ejemplos.

### 3.1. FROM

- **Propósito:** Especificar la imagen base sobre la que se construirá la nueva imagen.
- **Sintaxis:**
  ```dockerfile
  FROM <imagen>[:tag] [AS <nombre_etapa>]
  ```

- **Ejemplo Simple:**
  ```dockerfile
  FROM ubuntu:20.04
  ```

- **Ejemplo con Multi-stage (uso de alias):**
  ```dockerfile
  FROM node:14 AS builder
  # Se pueden realizar compilaciones o instalaciones de dependencias aquí
  
  FROM nginx:alpine
  COPY --from=builder /app/build /usr/share/nginx/html
  ```

### 3.2. LABEL

- **Propósito:** Añadir metadatos a la imagen (como autor, versión, descripción, etc.).
- **Sintaxis:**
  ```dockerfile
  LABEL clave="valor" otra_clave="otro_valor"
  ```

- **Ejemplo:**
  ```dockerfile
  LABEL maintainer="profesor@universidad.edu"
  LABEL version="1.0"
  ```

### 3.3. ARG

- **Propósito:** Definir variables que se usan durante la construcción de la imagen. Pueden ser sustituidas en tiempo de compilación.
- **Sintaxis:**
  ```dockerfile
  ARG NOMBRE
  ARG NOMBRE=valor_por_defecto
  ```

- **Ejemplo:**
  ```dockerfile
  ARG APP_VERSION=1.0.0
  RUN echo "Construyendo la versión ${APP_VERSION}"
  ```

*Nota:* Las variables definidas con `ARG` solo están disponibles durante la construcción, no en el contenedor en ejecución.

### 3.4. ENV

- **Propósito:** Configurar variables de entorno que estarán disponibles en tiempo de ejecución del contenedor.
- **Sintaxis:**
  ```dockerfile
  ENV NOMBRE valor
  ENV OTRA_NOMBRE=otro_valor
  ```

- **Ejemplo:**
  ```dockerfile
  ENV NODE_ENV production
  ENV APP_PORT 3000
  ```

### 3.5. RUN

- **Propósito:** Ejecutar comandos durante la construcción de la imagen. Cada comando `RUN` genera una capa.
- **Sintaxis:**
  ```dockerfile
  RUN comando
  ```
  Puede utilizarse en su forma shell o exec.

- **Ejemplo (Forma shell):**
  ```dockerfile
  RUN apt-get update && apt-get install -y curl git
  ```

- **Ejemplo (Forma exec):**
  ```dockerfile
  RUN ["apt-get", "update"]
  ```

*Consejo:* Utilizar la forma exec puede evitar problemas con la interpretación del shell y mejorar la seguridad.

### 3.6. COPY y ADD

- **Propósito:** Copiar archivos o directorios desde el contexto de construcción hacia la imagen.
- **Diferencia clave:**  
  - **COPY:** Simplemente copia archivos o directorios.
  - **ADD:** Tiene funcionalidades adicionales, como la descompresión de archivos comprimidos y el manejo de URLs.

- **Sintaxis:**
  ```dockerfile
  COPY origen destino
  ADD origen destino
  ```

- **Ejemplo con COPY:**
  ```dockerfile
  COPY . /app
  ```

- **Ejemplo con ADD (para un archivo tar):**
  ```dockerfile
  ADD archivo.tar.gz /app/
  ```

### 3.7. WORKDIR

- **Propósito:** Establecer el directorio de trabajo para las instrucciones siguientes.
- **Sintaxis:**
  ```dockerfile
  WORKDIR /ruta/del/directorio
  ```

- **Ejemplo:**
  ```dockerfile
  WORKDIR /app
  ```

### 3.8. EXPOSE

- **Propósito:** Indicar qué puertos utiliza la aplicación. Esta instrucción es informativa y no abre puertos en el host.
- **Sintaxis:**
  ```dockerfile
  EXPOSE puerto [puerto...]
  ```

- **Ejemplo:**
  ```dockerfile
  EXPOSE 80 443
  ```

### 3.9. ENTRYPOINT y CMD

Ambas instrucciones definen el comando que se ejecutará al iniciar el contenedor, pero tienen diferencias en su propósito y flexibilidad.

- **CMD:**
  - Se usa para definir el comando predeterminado.
  - Puede ser sobrescrito al ejecutar el contenedor.
  - Puede escribirse en formato shell o exec.

  **Ejemplo (Formato exec):**
  ```dockerfile
  CMD ["node", "app.js"]
  ```

  **Ejemplo (Formato shell):**
  ```dockerfile
  CMD node app.js
  ```

- **ENTRYPOINT:**
  - Define el ejecutable principal del contenedor.
  - Es más rígido, ya que los argumentos pasados al ejecutar el contenedor se añaden a este comando.
  - Puede combinarse con `CMD` para definir argumentos por defecto.

  **Ejemplo:**
  ```dockerfile
  ENTRYPOINT ["python"]
  CMD ["app.py"]
  ```
  Al ejecutar el contenedor, se utilizará `python app.py`. Si se pasan argumentos adicionales, estos se anexarán al comando definido en `ENTRYPOINT`.

### 3.10. Otras Instrucciones Relevantes

- **USER:** Especifica el usuario que ejecutará los comandos dentro del contenedor.
  ```dockerfile
  USER appuser
  ```

- **VOLUME:** Define un punto de montaje para volúmenes, útil para persistir datos.
  ```dockerfile
  VOLUME /data
  ```

- **HEALTHCHECK:** Permite definir un comando que verifica la salud del contenedor.
  ```dockerfile
  HEALTHCHECK --interval=30s --timeout=5s \
    CMD curl -f http://localhost/ || exit 1
  ```

## 4. Ejemplos Prácticos de Dockerfile

A continuación, se muestran varios ejemplos de Dockerfile que ilustran distintas opciones y configuraciones.

### 4.1. Ejemplo Básico

Este ejemplo crea una imagen simple basada en Ubuntu, instala algunas dependencias y define un comando predeterminado.

```dockerfile
# Imagen base
FROM ubuntu:20.04

# Metadatos
LABEL maintainer="profesor@universidad.edu"
LABEL description="Imagen básica para demostración"

# Variables de entorno
ENV DEBIAN_FRONTEND=noninteractive

# Actualizar el sistema e instalar dependencias
RUN apt-get update && apt-get install -y \
    curl \
    vim \
 && rm -rf /var/lib/apt/lists/*

# Establecer directorio de trabajo
WORKDIR /app

# Copiar archivos locales (si existen) al contenedor
COPY . /app

# Exponer puerto (informativo)
EXPOSE 8080

# Comando por defecto
CMD ["bash"]
```

### 4.2. Uso de ARG, ENV y Variables en la Construcción

Este ejemplo muestra cómo usar variables que permiten parametrizar la construcción de la imagen.

```dockerfile
# Definir un argumento para la versión de la aplicación
ARG APP_VERSION=1.0.0

FROM node:14

# Usar ARG durante la construcción
RUN echo "Construyendo la aplicación versión ${APP_VERSION}"

# Definir variables de entorno en tiempo de ejecución
ENV APP_VERSION=${APP_VERSION}
ENV NODE_ENV=production

WORKDIR /usr/src/app

# Copiar los archivos de la aplicación
COPY package*.json ./
RUN npm install

COPY . .

EXPOSE 3000

CMD ["node", "server.js"]
```

Para construir esta imagen con una versión distinta, se puede ejecutar:

```bash
docker build --build-arg APP_VERSION=2.0.0 -t mi_app:2.0.0 .
```

### 4.3. Multi-stage Build

Los multi-stage builds permiten reducir el tamaño final de la imagen utilizando etapas intermedias para compilar o procesar datos, y luego copiando únicamente los artefactos necesarios.

```dockerfile
# Etapa 1: Construcción de la aplicación
FROM golang:1.16 AS builder

WORKDIR /go/src/app
COPY . .

# Compilar la aplicación
RUN go build -o mi_aplicacion .

# Etapa 2: Imagen final ligera
FROM alpine:latest

WORKDIR /root/
# Copiar únicamente el ejecutable compilado desde la etapa "builder"
COPY --from=builder /go/src/app/mi_aplicacion .

# Definir el comando de inicio
CMD ["./mi_aplicacion"]
```

Este enfoque permite que la imagen final no incluya herramientas de compilación ni dependencias innecesarias, logrando así un tamaño más reducido y una mayor seguridad.

### 4.4. Ejemplo con ENTRYPOINT y CMD Combinados

Este ejemplo muestra cómo se puede fijar un comando base con `ENTRYPOINT` y definir argumentos por defecto con `CMD`.

```dockerfile
FROM python:3.9-slim

WORKDIR /usr/src/app

# Instalar dependencias
COPY requirements.txt ./
RUN pip install --no-cache-dir -r requirements.txt

COPY . .

# Definir el ENTRYPOINT
ENTRYPOINT ["python"]

# Definir el comando por defecto que se le pasará a ENTRYPOINT
CMD ["app.py"]
```

Al iniciar el contenedor, se ejecutará `python app.py`. Si se desea ejecutar otro script, se puede sobrescribir el CMD al ejecutar el contenedor, por ejemplo:

```bash
docker run -it mi_imagen otro_script.py
```


## 5. Conclusiones y Buenas Prácticas

El Dockerfile es una herramienta poderosa que permite automatizar la construcción de imágenes de Docker de forma reproducible y escalable. Algunas buenas prácticas a tener en cuenta son:

- **Orden Lógico:** Coloca las instrucciones en un orden que aproveche el cacheo de Docker. Por ejemplo, instala primero las dependencias que rara vez cambian y luego copia el código de la aplicación.
- **Minimizar Capas:** Combina comandos `RUN` cuando tenga sentido para reducir el número de capas y, por ende, el tamaño de la imagen.
- **Utilizar Multi-stage Builds:** Especialmente para aplicaciones compiladas, ayuda a generar imágenes finales más ligeras.
- **Etiquetar y Documentar:** Usa `LABEL` para añadir metadatos y comentarios en el Dockerfile para facilitar su mantenimiento y comprensión.
- **Variables y Parámetros:** Aprovecha `ARG` y `ENV` para parametrizar la construcción y configuración en tiempo de ejecución.
