### 2.4 Configuración Completa de un Proxy con Squid

**Squid** es un servidor proxy ampliamente utilizado que permite controlar y optimizar el tráfico de red. Su uso incluye funciones como la **caché de contenido web**, el **control de acceso** y la **filtración de URLs**, lo que ayuda a regular el acceso a internet y mejorar el rendimiento en redes corporativas.

#### 2.4.1 Introducción a Squid

**¿Qué es un Proxy?**

Un **proxy** es un servidor que actúa como intermediario entre los dispositivos de una red interna y el Internet. En lugar de conectarse directamente, los dispositivos envían sus solicitudes al proxy, que a su vez accede a los recursos en línea y los envía de vuelta a los clientes. Esto permite al administrador controlar qué recursos pueden o no acceder los usuarios y almacenar en caché contenido para reducir el consumo de ancho de banda.

**Tipos de Proxy y Usos de Squid**

Squid puede funcionar en varios modos:
- **Proxy Directo**: Intermediario para el acceso de clientes a recursos externos (internet).
- **Proxy Inverso**: Intermediario que protege y optimiza los accesos a un servidor web interno.
- **Caché de Contenidos**: Almacena en caché el contenido de sitios visitados frecuentemente para reducir el tráfico externo.

#### 2.4.2 Instalación y Configuración Inicial de Squid

Para comenzar a utilizar Squid, es necesario instalarlo y configurar su archivo principal de configuración: `/etc/squid/squid.conf`.

**Paso 1: Instalación de Squid**

1. **Actualizar Repositorios**:
   ```bash
   sudo apt update
   ```

2. **Instalar Squid**:
   ```bash
   sudo apt install squid -y
   ```

3. **Verificar el Estado del Servicio Squid**:
   ```bash
   sudo systemctl status squid
   ```
   Squid debería estar en ejecución después de la instalación.

**Paso 2: Configuración Básica de Squid**

Squid se configura mediante el archivo `/etc/squid/squid.conf`. Vamos a hacer algunos cambios iniciales para ajustar Squid a las necesidades básicas de la red.

1. **Abrir el Archivo de Configuración**:
   ```bash
   sudo nano /etc/squid/squid.conf
   ```

2. **Configurar el Puerto de Escucha**:
   - Por defecto, Squid escucha en el puerto `3128`. Para cambiarlo, busca la línea `http_port` y ajústala si es necesario:
     ```bash
     http_port 3128
     ```

3. **Permitir Acceso a la Red Local**:
   - Squid, por defecto, solo permite acceso desde el mismo servidor. Para permitir el acceso desde una red interna, agrega una regla para esa red:
     ```bash
     acl red_local src 192.168.1.0/24
     http_access allow red_local
     ```
     Esto permite que cualquier dispositivo en la subred `192.168.1.0/24` pueda usar el proxy.

4. **Reiniciar el Servicio para Aplicar Cambios**:
   ```bash
   sudo systemctl restart squid
   ```

#### 2.4.3 Configuración de Reglas Básicas de Acceso en Squid

A través de Squid, se pueden definir reglas para controlar el acceso a sitios y recursos web según el perfil de red deseado.

- **Permitir o Bloquear Acceso a Dominios Específicos**:
   1. En el archivo de configuración, define una regla ACL (Access Control List) para el dominio que quieres bloquear o permitir:
      ```bash
      acl bloqueado dstdomain .facebook.com .youtube.com
      http_access deny bloqueado
      ```
      Esto deniega el acceso a `facebook.com` y `youtube.com`.

   2. **Permitir Acceso Solo a Sitios Específicos**:
      ```bash
      acl permitidos dstdomain .educacion.com .instituto.edu
      http_access allow permitidos
      ```
      Esta regla permitirá que solo los sitios incluidos en la lista `permitidos` sean accesibles desde la red.

- **Configurar Acceso por Horarios**:
   - Squid permite definir políticas de acceso por horarios, útiles en entornos corporativos o educativos.
      ```bash
      acl horario_laboral time MTWHF 09:00-17:00
      http_access allow horario_laboral
      ```
      Esto permitirá el acceso a internet solo de lunes a viernes, entre las 9:00 y las 17:00.

#### 2.4.4 Configuración de Reglas Avanzadas de Control de Acceso en Squid

Squid ofrece configuraciones más avanzadas que permiten un control detallado del tráfico y acceso.

- **Filtrado de Acceso por IP o Rango de IP**:
   - Para limitar el acceso de una IP o grupo de IPs, usa reglas ACL basadas en direcciones IP:
     ```bash
     acl ip_restringida src 192.168.1.100
     http_access deny ip_restringida
     ```

- **Bloqueo de Extensiones de Archivo**:
   - Squid permite bloquear el acceso a ciertos tipos de archivo (por ejemplo, `.exe`, `.mp3`) mediante una lista de control:
     ```bash
     acl archivos_bloqueados url_regex -i \.exe$ \.mp3$
     http_access deny archivos_bloqueados
     ```
     Esto deniega la descarga de archivos con las extensiones `.exe` y `.mp3`.

- **Optimización del Caché en Squid**:
   - Para reducir el uso de ancho de banda, Squid puede configurarse para almacenar en caché contenido de sitios que se acceden con frecuencia.
     - Ejemplo de configuración de caché en el archivo `squid.conf`:
       ```bash
       cache_dir ufs /var/spool/squid 1000 16 256
       ```
       Esto configura Squid para almacenar 1000 MB de datos en caché en `/var/spool/squid`.

#### 2.4.5 Análisis de Logs en Squid

Squid genera logs que permiten supervisar y analizar el tráfico a través del proxy. Los logs se almacenan en `/var/log/squid/` y son útiles para detectar patrones de uso y evaluar la efectividad de las reglas aplicadas.

- **Revisar los Logs de Acceso de Squid**:
   ```bash
   tail -f /var/log/squid/access.log
   ```
   Este archivo registra cada solicitud que pasa por el proxy, incluyendo la dirección IP de origen, la URL solicitada y el tiempo de respuesta.

- **Buscar Accesos Bloqueados**:
   - Para ver solo las líneas que contienen accesos bloqueados, usa `grep`:
     ```bash
     grep "TCP_DENIED" /var/log/squid/access.log
     ```

#### 2.4.6 Casos Prácticos con Squid

##### Ejemplo 1: Configurar Squid para una Red de Oficina con Acceso Controlado

1. **Permitir Acceso Solo a Sitios de Trabajo**:
   - Editar `squid.conf` para definir una lista de sitios permitidos:
     ```bash
     acl sitios_trabajo dstdomain .empresa.com .soporte.com
     http_access allow sitios_trabajo
     ```
   - Denegar todo el tráfico restante:
     ```bash
     http_access deny all
     ```

2. **Permitir Acceso Solo en Horario Laboral**:
   - Añadir la restricción de tiempo:
     ```bash
     acl horario_laboral time MTWHF 08:00-18:00
     http_access allow horario_laboral
     ```

##### Ejemplo 2: Crear un Proxy Inverso con Squid para Mejorar la Seguridad de un Servidor Web

1. **Configurar Squid como Proxy Inverso**:
   - Cambia el puerto de escucha de Squid para que reciba las solicitudes de los usuarios hacia el servidor web.
     ```bash
     http_port 80 accel defaultsite=servidorweb.com
     cache_peer servidorweb.com parent 80 0 no-query originserver
     ```
   - Esto redirige el tráfico hacia el servidor web interno, protegiéndolo de accesos directos.

##### Ejemplo 3: Implementar un Sistema de Caché en Squid para Optimizar el Ancho de Banda

1. **Configurar Caché en el Proxy**:
   - Configurar la caché en `squid.conf` para optimizar el almacenamiento de contenido:
     ```bash
     cache_dir ufs /var/spool/squid 2000 16 256
     maximum_object_size 50 MB
     cache_mem 256 MB
     ```

2. **Habilitar la Caché para Todos los Sitios**:
   - Permitir que el proxy almacene en caché todos los sitios web accedidos por los usuarios, lo que reducirá el consumo de ancho de banda en el acceso recurrente a estos sitios.

---
