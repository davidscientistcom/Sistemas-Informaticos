## Manual para Entender Docker Compose

Docker Compose es una herramienta muy útil dentro del ecosistema Docker. Permite definir y gestionar aplicaciones multi-contenedor de una manera sencilla y eficiente. En lugar de tener que iniciar y configurar cada contenedor individualmente con comandos `docker run` largos y complejos, Docker Compose utiliza un archivo de configuración (normalmente llamado `docker-compose.yml`) para describir todos los servicios (contenedores) que forman tu aplicación y cómo se relacionan entre sí.

**Imagina que tu aplicación está compuesta por varias piezas:** una base de datos, un servidor web y una aplicación backend. Con Docker Compose, puedes definir todas estas piezas en un solo archivo y luego, con un solo comando, Docker se encarga de crear y ejecutar todos los contenedores necesarios.

### Conceptos Clave de Docker Compose

* **`docker-compose.yml`:** Este es el archivo principal de configuración. En él defines los diferentes servicios (contenedores) que componen tu aplicación, la imagen Docker que deben usar, las configuraciones de red, los volúmenes, las variables de entorno, etc.
* **Servicio (Service):** Un servicio representa un contenedor que se va a ejecutar. En el archivo `docker-compose.yml`, defines cada servicio con su respectiva configuración (imagen, puertos, etc.).
* **Proyecto:** Un proyecto en Docker Compose es un conjunto de servicios relacionados definidos en un archivo `docker-compose.yml`.

### Ejemplo Básico: Un Contenedor con Docker Compose

Antes de ver cómo ejecutar múltiples contenedores de la misma imagen, veamos un ejemplo sencillo con un solo contenedor para entender la estructura del archivo `docker-compose.yml`.

Supongamos que queremos ejecutar un contenedor de la imagen oficial de Nginx.

1.  **Crea un directorio para tu proyecto:**

    ```bash
    mkdir mi_proyecto_nginx
    cd mi_proyecto_nginx
    ```

2.  **Crea un archivo llamado `docker-compose.yml` dentro de este directorio:**

    ```yaml
    version: '3.8'  # Especifica la versión del formato del archivo Docker Compose

    services:
      web:          # Define un servicio llamado 'web'
        image: nginx:latest  # Indica la imagen Docker que se va a usar (la última versión de Nginx)
        ports:
          - "80:80"    # Mapea el puerto 80 del host al puerto 80 del contenedor
    ```

3.  **Ejecuta el comando para iniciar el contenedor:**

    ```bash
    docker-compose up -d
    ```

    * `docker-compose up`: Este comando crea e inicia los contenedores definidos en el archivo `docker-compose.yml`.
    * `-d`: Este flag indica que se ejecute en modo "detached" (en segundo plano).

4.  **Verifica que el contenedor se está ejecutando:**

    ```bash
    docker ps
    ```

    Deberías ver un contenedor con el nombre (aproximado) `mi_proyecto_nginx-web-1` ejecutándose.

5.  **Accede a Nginx:** Abre tu navegador y ve a `http://localhost`. Deberías ver la página de bienvenida de Nginx.

6.  **Para detener y eliminar el contenedor:**

    ```bash
    docker-compose down
    ```

### Ejecutando Múltiples Contenedores de la Misma Imagen

Ahora vamos al punto que te interesa: cómo ejecutar múltiples contenedores utilizando la misma imagen Docker. La clave está en definir múltiples servicios en tu archivo `docker-compose.yml`, cada uno utilizando la misma imagen pero con configuraciones potencialmente diferentes (como diferentes puertos, nombres de contenedor, variables de entorno, etc.).

**Escenario:** Imagina que tienes una aplicación web simple empaquetada en una imagen Docker llamada `mi_app_web`. Quieres ejecutar tres instancias de esta aplicación, cada una accesible en un puerto diferente de tu máquina local.

1.  **Crea un directorio para tu proyecto:**

    ```bash
    mkdir multi_instancias
    cd multi_instancias
    ```

2.  **Crea un archivo llamado `docker-compose.yml` dentro de este directorio:**

    ```yaml
    version: '3.8'

    services:
      web1:         # Define la primera instancia de la aplicación web
        image: mi_app_web:latest  # Reemplaza 'mi_app_web:latest' con el nombre real de tu imagen
        ports:
          - "8080:80"    # Mapea el puerto 8080 del host al puerto 80 del contenedor

      web2:         # Define la segunda instancia de la aplicación web
        image: mi_app_web:latest
        ports:
          - "8081:80"    # Mapea el puerto 8081 del host al puerto 80 del contenedor

      web3:         # Define la tercera instancia de la aplicación web
        image: mi_app_web:latest
        ports:
          - "8082:80"    # Mapea el puerto 8082 del host al puerto 80 del contenedor
    ```

    **Explicación:**

    * Hemos definido tres servicios diferentes: `web1`, `web2` y `web3`.
    * Cada servicio utiliza la misma imagen: `mi_app_web:latest`.
    * Cada servicio mapea un puerto diferente de tu máquina local a un puerto dentro del contenedor (en este caso, asumimos que tu aplicación web escucha en el puerto 80 dentro del contenedor).
        * `web1` estará accesible en `http://localhost:8080`
        * `web2` estará accesible en `http://localhost:8081`
        * `web3` estará accesible en `http://localhost:8082`

3.  **Ejecuta el comando para iniciar los contenedores:**

    ```bash
    docker-compose up -d
    ```

4.  **Verifica que los contenedores se están ejecutando:**

    ```bash
    docker ps
    ```

    Deberías ver tres contenedores ejecutándose, cada uno con un nombre similar a `multi_instancias-web1-1`, `multi_instancias-web2-1` y `multi_instancias-web3-1`, y con los puertos mapeados como se definieron.

5.  **Accede a las diferentes instancias de tu aplicación:** Abre tu navegador y ve a:
    * `http://localhost:8080`
    * `http://localhost:8081`
    * `http://localhost:8082`

    Deberías ver la misma aplicación web ejecutándose en cada una de estas URLs, pero en contenedores separados.

6.  **Para detener y eliminar los contenedores:**

    ```bash
    docker-compose down
    ```

### Variaciones y Consideraciones

* **Nombres de Contenedor:** Docker Compose automáticamente asigna nombres a los contenedores basados en el nombre del proyecto y el nombre del servicio. Puedes personalizar los nombres de los contenedores si lo necesitas utilizando la opción `container_name` dentro de la definición de cada servicio. Sin embargo, para este caso de múltiples instancias, los nombres generados automáticamente suelen ser suficientes para distinguirlos.

    ```yaml
    version: '3.8'

    services:
      web1:
        image: mi_app_web:latest
        ports:
          - "8080:80"
        container_name: mi_aplicacion_web_instancia_1

      # ... y así sucesivamente para web2 y web3
    ```

* **Variables de Entorno:** Si necesitas configurar cada instancia de tu aplicación de manera diferente, puedes utilizar la opción `environment` para definir variables de entorno específicas para cada servicio.

    ```yaml
    version: '3.8'

    services:
      web1:
        image: mi_app_web:latest
        ports:
          - "8080:80"
        environment:
          - INSTANCE_ID=1
          - CONFIG_FILE=/etc/config/web1.conf

      web2:
        image: mi_app_web:latest
        ports:
          - "8081:80"
        environment:
          - INSTANCE_ID=2
          - CONFIG_FILE=/etc/config/web2.conf

      # ...
    ```

* **Volúmenes:** Si tus aplicaciones necesitan acceder a datos persistentes o compartir archivos, puedes definir volúmenes para cada servicio.

    ```yaml
    version: '3.8'

    services:
      web1:
        image: mi_app_web:latest
        ports:
          - "8080:80"
        volumes:
          - ./data1:/app/data

      web2:
        image: mi_app_web:latest
        ports:
          - "8081:80"
        volumes:
          - ./data2:/app/data

      # ...
    ```

### Conclusión

Docker Compose simplifica enormemente la gestión de aplicaciones multi-contenedor. Para ejecutar múltiples instancias de la misma imagen, simplemente defines múltiples servicios en tu archivo `docker-compose.yml`, cada uno utilizando la misma imagen pero con configuraciones únicas según tus necesidades (principalmente diferentes puertos para evitar conflictos).