Claro, ahora seguimos con la **Sección 2.3: Configuración Completa de un Firewall con UFW**. Aquí proporcionaremos una guía detallada sobre el uso de UFW, abarcando desde la configuración básica hasta reglas avanzadas y ejemplos prácticos.

---

### 2.3 Configuración Completa de un Firewall con UFW

El **Uncomplicated Firewall (UFW)** es una interfaz simplificada para gestionar `iptables`, el firewall de Linux. UFW está diseñado para hacer más accesible la configuración del firewall, permitiendo controlar de manera segura el tráfico de red que entra y sale del sistema.

#### 2.3.1 Introducción a UFW

**¿Qué es UFW?**

UFW es una herramienta que facilita la configuración de un firewall en sistemas Linux, especialmente en distribuciones como Ubuntu. A través de UFW, se pueden definir reglas para permitir o bloquear tráfico hacia y desde el sistema, asegurando que solo el tráfico autorizado pueda acceder a los servicios.

**Relación de UFW con `iptables`**

Detrás de UFW, el sistema sigue utilizando `iptables`, que es el módulo del kernel que gestiona el filtrado de paquetes en Linux. UFW se encarga de traducir las reglas definidas en una interfaz simplificada a comandos de `iptables`, facilitando la administración del firewall.

**Estructura General y Funcionamiento**

El firewall en Linux funciona evaluando cada paquete de red entrante o saliente según un conjunto de reglas predefinidas. Las reglas de UFW permiten especificar qué puertos, direcciones IP, protocolos y servicios están permitidos o denegados en la red.

#### 2.3.2 Configuración Inicial del Firewall con UFW

Antes de definir reglas de firewall, es necesario activar UFW y familiarizarse con algunos comandos básicos.

- **Activar UFW**: Esto enciende el firewall en el sistema.
   ```bash
   sudo ufw enable
   ```
   Nota: Al activar UFW, el tráfico hacia el sistema será controlado automáticamente. 

- **Desactivar UFW**: Este comando detiene el firewall y elimina las restricciones.
   ```bash
   sudo ufw disable
   ```

- **Permitir Conexiones**: Se pueden habilitar puertos o servicios específicos.
   - Permitir conexiones en el puerto SSH (22) para el protocolo TCP:
     ```bash
     sudo ufw allow 22/tcp
     ```
   - Permitir conexiones HTTP (puerto 80) y HTTPS (puerto 443):
     ```bash
     sudo ufw allow 80
     sudo ufw allow 443
     ```

- **Denegar Conexiones**: UFW también permite bloquear el acceso a servicios o puertos.
   - Denegar acceso al puerto 21 (FTP):
     ```bash
     sudo ufw deny 21
     ```

- **Verificar el Estado del Firewall**: Comando para revisar todas las reglas activas.
   ```bash
   sudo ufw status verbose
   ```
   La salida muestra las reglas activas, especificando el puerto, el protocolo y si la conexión está permitida o denegada.

#### 2.3.3 Reglas de Firewall Detalladas

**Reglas Básicas**

- **Permitir y Denegar Puertos Individuales**:
   - Ejemplo: Permitir el puerto 3306 para MySQL:
     ```bash
     sudo ufw allow 3306
     ```
   - Ejemplo: Denegar el puerto 23 (Telnet):
     ```bash
     sudo ufw deny 23
     ```

- **Rangos de Puertos**:
   - Para habilitar un rango de puertos, como del 1000 al 2000:
     ```bash
     sudo ufw allow 1000:2000/tcp
     ```

- **Control de Conexiones Entrantes y Salientes**:
   - Permitir conexiones entrantes al puerto 22 (SSH):
     ```bash
     sudo ufw allow in 22/tcp
     ```
   - Bloquear conexiones salientes en el puerto 25 (SMTP):
     ```bash
     sudo ufw deny out 25/tcp
     ```

**Reglas Avanzadas**

- **Filtrado por IP o Rango de IP**:
   - Permitir solo a una IP específica el acceso SSH:
     ```bash
     sudo ufw allow from 192.168.1.10 to any port 22
     ```
   - Bloquear acceso HTTP desde una red específica (192.168.2.0/24):
     ```bash
     sudo ufw deny from 192.168.2.0/24 to any port 80
     ```

- **Control de Tráfico por Protocolo**:
   - Permitir solo tráfico UDP desde una red interna:
     ```bash
     sudo ufw allow proto udp from 10.0.0.0/8
     ```

**Control de Tráfico por Interfaces**

Algunas configuraciones requieren aplicar reglas a interfaces de red específicas. Esto es común en servidores que cuentan con varias interfaces.

- **Permitir tráfico HTTP solo en la interfaz `eth0`**:
   ```bash
   sudo ufw allow in on eth0 to any port 80
   ```
- **Bloquear tráfico SSH en la interfaz `eth1`**:
   ```bash
   sudo ufw deny in on eth1 to any port 22
   ```

#### 2.3.4 Logs de UFW y Monitorización de Tráfico

UFW permite activar la generación de logs para registrar eventos relacionados con el firewall. Esto es útil para identificar intentos de acceso no autorizados y analizar el tráfico en la red.

- **Activar Logging de UFW**:
   ```bash
   sudo ufw logging on
   ```
   Esto habilita el registro de eventos de UFW en el archivo `/var/log/ufw.log`.

- **Análisis de los Registros de UFW**:
   - **Ver los últimos eventos de UFW**:
     ```bash
     tail /var/log/ufw.log
     ```
   - **Buscar eventos específicos**:
     - Filtrar eventos de bloqueo en los últimos 100 eventos:
       ```bash
       tail -n 100 /var/log/ufw.log | grep "BLOCK"
       ```

   A través de estos logs, los administradores pueden identificar IPs que intentan acceder repetidamente y patrones de tráfico inusuales, ayudando en la detección de amenazas.

#### 2.3.5 Casos Prácticos con UFW

A continuación, se presentan casos prácticos para que los estudiantes apliquen las reglas y comandos aprendidos en un contexto real.

##### Ejemplo 1: Crear un Firewall para un Servidor Web con Acceso Seguro al Puerto SSH

1. **Permitir Acceso SSH solo desde una IP específica**:
   ```bash
   sudo ufw allow from 203.0.113.10 to any port 22
   ```
2. **Permitir Acceso HTTP y HTTPS para Todos**:
   ```bash
   sudo ufw allow 80
   sudo ufw allow 443
   ```
3. **Bloquear Todo el Tráfico Restante**:
   ```bash
   sudo ufw default deny incoming
   ```

##### Ejemplo 2: Configurar Reglas de Firewall en un Entorno de Red Local

1. **Permitir acceso al puerto 80 solo dentro de la red interna (192.168.1.0/24)**:
   ```bash
   sudo ufw allow from 192.168.1.0/24 to any port 80
   ```
2. **Bloquear acceso al puerto SSH desde todas las direcciones excepto la red interna**:
   ```bash
   sudo ufw deny in on eth0 to any port 22
   sudo ufw allow from 192.168.1.0/24 to any port 22
   ```

##### Ejemplo 3: Bloquear un Ataque de Fuerza Bruta en el Puerto SSH

1. **Detectar Intentos de Acceso en el Log**:
   - Verificar el log para ver qué IPs intentan acceder repetidamente:
     ```bash
     tail -f /var/log/auth.log | grep "Failed password"
     ```

2. **Bloquear la IP Problemática**:
   - Si detectas una IP que intenta realizar un ataque de fuerza bruta:
     ```bash
     sudo ufw deny from 192.168.1.100 to any port 22
     ```

3. **Configurar Reglas para Solo IPs de Confianza**:
   - Permitir solo conexiones SSH desde IPs de confianza, bloqueando el resto.
     ```bash
     sudo ufw allow from 203.0.113.0/24 to any port 22
     sudo ufw default deny incoming
     ```

---

### Resumen

En esta sección, hemos explorado UFW en detalle, desde su configuración inicial hasta la implementación de reglas avanzadas y ejemplos prácticos. Los estudiantes ahora tienen una base sólida para aplicar y gestionar un firewall en Ubuntu, garantizando la seguridad de los servicios en red y la administración eficaz del tráfico mediante un enfoque estructurado y accesible. 

Con UFW configurado, la siguiente sección abordará la instalación y configuración de **Squid** como un proxy, permitiendo controlar el acceso a internet y monitorizar el uso de red en el entorno de pruebas.