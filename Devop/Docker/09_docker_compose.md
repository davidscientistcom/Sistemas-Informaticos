### Tutorial
https://imaginaformacion.com/tutoriales/que-es-docker-compose


## Manual Introductorio de Docker Compose

Cómo Ejecutar Múltiples Contenedores desde una Misma Imagen

¿Qué es Docker Compose?

Docker Compose es una herramienta oficial del ecosistema Docker que permite definir, configurar y ejecutar aplicaciones formadas por múltiples contenedores. En lugar de lanzar manualmente cada contenedor con comandos docker run, Compose usa un único archivo (docker-compose.yml) donde se describe cómo deben crearse, configurarse y relacionarse todos los servicios (contenedores) que componen tu aplicación.

¿Por qué es útil?

Piensa en una aplicación real: normalmente no se limita a un solo componente. Puede tener:
	•	Una base de datos (como PostgreSQL),
	•	Un servidor backend (como una API en Python),
	•	Un servidor frontend (como una app React).

Con Docker Compose puedes definir estos tres servicios en un solo archivo y levantarlos todos juntos con un único comando.

### Conceptos Clave

#### Concepto	Descripción
docker-compose.yml	Archivo donde defines todos los servicios, redes, volúmenes y configuraciones necesarias.
Servicio (service)	Cada servicio representa un contenedor con su propia configuración (imagen, puertos, volúmenes, variables de entorno, etc.).
Proyecto	Todo el conjunto definido por un docker-compose.yml. El nombre del proyecto suele ser el nombre del directorio que contiene el archivo YAML.

Ejemplo Básico: Lanzando un Contenedor con Docker Compose

  #### Objetivo

Levantar un servidor NGINX usando Docker Compose para entender la estructura básica del archivo docker-compose.yml.

Pasos
	1.	Crear el directorio del proyecto:

```bash
mkdir mi_proyecto_nginx
cd mi_proyecto_nginx
```

	2.	Crear el archivo docker-compose.yml:

```yaml
version: '3.8'

services:
  web:
    image: nginx:latest
    ports:
      - "80:80"
```

	3.	Iniciar el contenedor:

```bash
docker-compose up -d
```

	4.	Verificar su ejecución:

```bash
docker ps
```

	5.	Acceder desde el navegador:
Visita http://localhost
	6.	Detener y eliminar el contenedor:

```bash
docker-compose down
```

Caso de Uso: Múltiples Contenedores de la Misma Imagen

Escenario

Dispones de una imagen llamada mi_app_web y quieres lanzar tres instancias independientes que se comporten como réplicas, accesibles desde diferentes puertos locales.


Estructura del Proyecto
	1.	Crear carpeta del proyecto:

mkdir multi_instancias
cd multi_instancias


	2.	Archivo docker-compose.yml:

``` yaml
version: '3.8'

services:
  web1:
    image: mi_app_web:latest
    ports:
      - "8080:80"

  web2:
    image: mi_app_web:latest
    ports:
      - "8081:80"

  web3:
    image: mi_app_web:latest
    ports:
      - "8082:80"
```

Ejecución
	1.	Lanzar los tres contenedores:

```bash
docker-compose up -d
```

	2.	Verificarlos:

docker ps

Verás los contenedores multi_instancias_web1_1, web2_1, web3_1, cada uno escuchando en un puerto distinto.

	3.	Acceder a cada instancia:
	•	http://localhost:8080
	•	http://localhost:8081
	•	http://localhost:8082
	4.	Apagar todos los contenedores:

```bash
docker-compose down
```



## Personalización Avanzada

1. Nombres Personalizados para Contenedores

Si prefieres nombres explícitos:

```yaml
services:
  web1:
    image: mi_app_web:latest
    ports:
      - "8080:80"
    container_name: instancia_web_1
```




#### Variables de Entorno por Instancia

Útil si tu aplicación lee una configuración diferente en función de las variables.

```yaml
services:
  web1:
    image: mi_app_web:latest
    ports:
      - "8080:80"
    environment:
      - INSTANCE_ID=1
      - CONFIG_FILE=/etc/config/web1.conf
```



#### Montaje de Volúmenes

Ideal para mantener datos persistentes o configuraciones distintas por instancia.

```yaml
services:
  web1:
    image: mi_app_web:latest
    ports:
      - "8080:80"
    volumes:
      - ./data1:/app/data
```

