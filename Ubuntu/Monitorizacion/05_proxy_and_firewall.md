### Arquitectura de Red para Combinar Proxy y Firewall

**Objetivo**: Diseñar una arquitectura en la cual todas las conexiones de la red interna a internet sean gestionadas primero por el **firewall** (UFW) y luego por el **proxy** (Squid), que controlará y filtrará el tráfico de salida.

### Componentes y Roles

1. **Máquina 1: Servidor de Proxy y Firewall (Squid + UFW)**  
   - **Rol**: Esta máquina gestionará el tráfico de la red interna a internet mediante reglas de firewall (UFW) y el control de acceso a través de un proxy (Squid).
   - **Red**: Conectada a dos interfaces de red:
     - **Interfaz Interna (eth0)**: Conectada a la red interna (por ejemplo, `192.168.1.0/24`).
     - **Interfaz Externa (eth1)**: Conectada directamente a internet o a una red externa.

2. **Máquina 2: Clientes de la Red Interna**  
   - **Rol**: Simulan dispositivos de usuario que se conectan a internet a través del servidor de proxy y firewall.
   - **Red**: Conectada a la red interna (por ejemplo, `192.168.1.0/24`), configurada para utilizar el proxy como puerta de enlace a internet.

### Flujo de Tráfico

1. **Control de Tráfico Entrante con UFW**:
   - UFW se configura en la **Máquina 1** (proxy/firewall) para gestionar todo el tráfico entrante y saliente. Las reglas de UFW permitirán únicamente las conexiones que se inician desde la red interna hacia el proxy y luego hacia internet.
   - Las reglas de UFW, aplicadas en la interfaz interna (`eth0`), controlan el tráfico de los clientes internos y aseguran que solo se pueda acceder a internet a través del proxy.

2. **Filtrado y Control de Acceso con Squid**:
   - Squid en la **Máquina 1** actúa como proxy, aceptando solicitudes HTTP/HTTPS de los clientes internos. Squid se encarga de la **caché de contenidos** y del **control de acceso a URLs y dominios**.
   - Al estar Squid configurado como intermediario, permite o bloquea el acceso a internet según las políticas definidas (como la lista de sitios permitidos o bloqueados).
   
### Configuración General

1. **Configuración del Proxy (Squid) en Máquina 1**  
   - Instala y configura Squid en la **Máquina 1** para aceptar conexiones de la red interna y gestionar el tráfico de navegación.
   - Configura Squid para que almacene en caché los sitios permitidos y bloquee sitios no autorizados.

2. **Configuración del Firewall (UFW) en Máquina 1**  
   - Configura UFW en **Máquina 1** para permitir solo tráfico entrante en el puerto del proxy (3128, por ejemplo) desde la red interna.
   - Aplica reglas de firewall para bloquear todo tráfico no autorizado desde la red interna hacia internet si no pasa por el proxy.

3. **Configuración de Clientes en la Red Interna**  
   - Configura cada cliente en la **Máquina 2** para utilizar la **Máquina 1** como servidor proxy. Puedes definir esta configuración a nivel de sistema o navegador para que todo el tráfico HTTP/HTTPS se dirija al proxy Squid.

### Ejemplo de Configuración

1. **Reglas de Firewall en Máquina 1**:
   - Permitir tráfico desde la red interna hacia el proxy en el puerto de Squid (3128).
     ```bash
     sudo ufw allow from 192.168.1.0/24 to any port 3128
     ```
   - Bloquear todo el tráfico directo desde la red interna a internet.
     ```bash
     sudo ufw deny out from 192.168.1.0/24 to any
     ```
   - Permitir solo el tráfico que proviene del proxy hacia internet en la interfaz externa (`eth1`).
     ```bash
     sudo ufw allow out on eth1 from any to any
     ```

2. **Configuración Básica de Squid en Máquina 1**:
   - En el archivo `/etc/squid/squid.conf`, configura el proxy para aceptar conexiones desde la red interna (`192.168.1.0/24`).
     ```bash
     acl red_interna src 192.168.1.0/24
     http_access allow red_interna
     ```
   - Define una lista de sitios permitidos o bloqueados según las necesidades.

3. **Configuración de los Clientes en Máquina 2**:
   - Configura cada cliente de la red interna para usar la **Máquina 1** como su proxy en el puerto `3128`.

### Ventajas de esta Arquitectura

- **Control Centralizado**: Todo el tráfico hacia internet pasa por una única máquina, lo que facilita la administración y la seguridad.
- **Filtrado Doble**: UFW limita el tráfico solo al proxy, y Squid gestiona el control de accesos, brindando un control completo del tráfico saliente.
- **Caché de Contenidos**: Squid permite optimizar el ancho de banda mediante el almacenamiento en caché de contenidos, lo cual mejora el rendimiento en redes con varios clientes.
