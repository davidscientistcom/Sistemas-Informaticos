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



##  Redes en Docker Compose

Docker proporciona un sistema de redes que permite a los contenedores comunicarse entre sí y con el mundo exterior. Docker Compose simplifica la gestión de estas redes cuando trabajas con múltiples contenedores definidos en tu archivo `docker-compose.yml`.

### Red Predeterminada en Docker Compose

Cuando ejecutas `docker-compose up`, Docker Compose automáticamente crea una red predeterminada para tu proyecto. Todos los servicios (contenedores) definidos en el mismo archivo `docker-compose.yml` se unen automáticamente a esta red. Esto permite que los contenedores se descubran y se comuniquen entre sí utilizando sus nombres de servicio como nombres de host.

**Ejemplo:**

Considera el ejemplo anterior con tres instancias de `mi_app_web`. Aunque no definimos explícitamente una red, Docker Compose creó una red para el proyecto `multi_instancias`. Si tuvieras otro servicio en el mismo `docker-compose.yml` (por ejemplo, un balanceador de carga), ese servicio también estaría en la misma red y podría comunicarse con `web1`, `web2` y `web3` utilizando sus nombres de servicio.

### Comunicación entre Contenedores

Dentro de la red predeterminada creada por Docker Compose, los contenedores pueden comunicarse entre sí utilizando el nombre del servicio como si fuera un nombre de host. Docker tiene un servidor DNS interno que resuelve los nombres de servicio a las direcciones IP de los contenedores correspondientes.

**Ejemplo:**

Imagina que tienes dos servicios en tu `docker-compose.yml`: una aplicación web (`web`) y una base de datos (`db`).

```yaml
version: '3.8'

services:
  web:
    image: mi_app_web:latest
    ports:
      - "80:80"
    depends_on:
      - db  # Indica que 'web' depende de que 'db' esté en funcionamiento

  db:
    image: postgres:latest
    environment:
      POSTGRES_PASSWORD: mysecretpassword
```

Dentro del código de tu aplicación web (`mi_app_web`), podrías conectarte a la base de datos utilizando el nombre de host `db` (y el puerto por defecto de PostgreSQL, que es 5432). Docker resolverá automáticamente `db` a la dirección IP del contenedor de la base de datos.

### Mapeo de Puertos (`ports`) y la Red

La sección `ports` en la definición de un servicio se utiliza para exponer puertos del contenedor a la máquina host. Es importante entender que esto es diferente de la comunicación dentro de la red de Docker Compose.

* **`"80:80"`:** Esto significa que el puerto 80 del contenedor estará accesible en el puerto 80 de la máquina donde se está ejecutando Docker Compose. Esto es lo que permite acceder a tu aplicación desde tu navegador utilizando `localhost:80`.
* **Comunicación Interna:** Los otros servicios dentro de la misma red pueden acceder al servicio en su puerto interno (en este caso, el puerto 80) utilizando su nombre de servicio, sin necesidad de que el puerto esté expuesto a la máquina host.

En el ejemplo de las tres instancias de `mi_app_web`, cada instancia expone un puerto diferente al host (8080, 8081, 8082), pero internamente, dentro de la red `multi_instancias`, todas las instancias se ejecutan y escuchan en el puerto 80. Si otro servicio dentro de esta red quisiera comunicarse con `web1`, lo haría a través del puerto 80 del contenedor `web1` utilizando el nombre de servicio `web1`.

### Definición de Redes Personalizadas

Aunque la red predeterminada suele ser suficiente para la mayoría de los casos, Docker Compose te permite definir redes personalizadas si necesitas un mayor control sobre la topología de la red.

Puedes definir redes en la sección `networks` de tu archivo `docker-compose.yml`.

**Ejemplo:**

Supongamos que quieres tener una red separada para la comunicación interna entre tu aplicación web y tu base de datos, y otra red para un balanceador de carga que solo necesita acceder a las instancias de la aplicación web.

```yaml
version: '3.8'

services:
  web1:
    image: mi_app_web:latest
    ports:
      - "8080:80"
    networks:
      - frontend
      - backend

  web2:
    image: mi_app_web:latest
    ports:
      - "8081:80"
    networks:
      - frontend
      - backend

  db:
    image: postgres:latest
    environment:
      POSTGRES_PASSWORD: mysecretpassword
    networks:
      - backend

  loadbalancer:
    image: nginx:latest
    ports:
      - "80:80"
    depends_on:
      - web1
      - web2
    networks:
      - frontend
    volumes:
      - ./nginx.conf:/etc/nginx/nginx.conf:ro

networks:
  frontend:
    driver: bridge
  backend:
    driver: bridge
```

**Explicación:**

* Hemos definido dos redes personalizadas: `frontend` y `backend`, ambas utilizando el driver `bridge` (que es el driver predeterminado).
* Los servicios `web1` y `web2` están conectados a ambas redes.
* El servicio `db` solo está conectado a la red `backend`.
* El servicio `loadbalancer` solo está conectado a la red `frontend`.

Esto significa que:

* `web1` y `web2` pueden comunicarse entre sí y con el `loadbalancer` a través de la red `frontend`, y también pueden comunicarse con la `db` a través de la red `backend`.
* La `db` solo puede ser accedida por los servicios que estén en la red `backend` (en este caso, `web1` y `web2`).
* El `loadbalancer` solo puede acceder a `web1` y `web2` a través de la red `frontend`.

Esta configuración proporciona un mayor aislamiento y control sobre el flujo de red entre tus contenedores.

### Conectando Servicios a Redes

La sección `networks` dentro de la definición de un servicio se utiliza para especificar a qué redes debe conectarse ese servicio. Puedes listar los nombres de las redes definidas en la sección `networks` de nivel superior.

Si no se especifica ninguna red para un servicio, se conectará automáticamente a la red predeterminada creada por Docker Compose.

### Drivers de Red

Docker soporta diferentes drivers de red. El driver `bridge` es el predeterminado y crea una red privada dentro del host donde los contenedores pueden comunicarse. Otros drivers incluyen `host` (que omite el aislamiento de red entre el contenedor y el host) y `overlay` (utilizado para redes multi-host). Para la mayoría de los casos de desarrollo y despliegues en un solo host, el driver `bridge` es suficiente.

### Resolución DNS

Como mencioné anteriormente, Docker Compose utiliza un servidor DNS interno. Esto significa que dentro de la red de Docker Compose, puedes utilizar el nombre del servicio como un nombre de host para acceder a ese servicio. Por ejemplo, si tienes un servicio llamado `mi_servicio`, otros contenedores en la misma red pueden acceder a él utilizando el nombre de host `mi_servicio`.

### Ejemplo Completo con Redes

Vamos a modificar el ejemplo de las tres instancias de `mi_app_web` para incluir una red personalizada.

1.  **Crea o edita el archivo `docker-compose.yml`:**

    ```yaml
    version: '3.8'

    services:
      web1:
        image: mi_app_web:latest
        ports:
          - "8080:80"
        networks:
          - mi_red_web

      web2:
        image: mi_app_web:latest
        ports:
          - "8081:80"
        networks:
          - mi_red_web

      web3:
        image: mi_app_web:latest
        ports:
          - "8082:80"
        networks:
          - mi_red_web

    networks:
      mi_red_web:
        driver: bridge
    ```

2.  **Ejecuta el comando para iniciar los contenedores:**

    ```bash
    docker-compose up -d
    ```

En este caso, hemos creado una red personalizada llamada `mi_red_web` y hemos asignado las tres instancias de nuestra aplicación web a esta red. La funcionalidad en términos de acceso desde el host sigue siendo la misma (a través de los puertos mapeados), pero ahora los contenedores están explícitamente dentro de una red definida por nosotros.

