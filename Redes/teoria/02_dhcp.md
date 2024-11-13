### DHCP: Protocolo de Configuración Dinámica de Host

El **Protocolo de Configuración Dinámica de Host** o **DHCP** (por sus siglas en inglés, Dynamic Host Configuration Protocol) es un protocolo de red que permite asignar de forma automática y dinámica direcciones IP y otros parámetros de configuración necesarios para la conexión de dispositivos a una red. DHCP simplifica la administración de redes al proporcionar automáticamente a cada dispositivo una configuración sin necesidad de intervención manual.

#### Funcionamiento de DHCP

DHCP se basa en un modelo cliente-servidor. En este modelo, un servidor DHCP tiene la tarea de asignar direcciones IP y otros parámetros de configuración a los dispositivos que se conectan a la red (los "clientes"). El proceso para asignar una IP a un dispositivo implica varias etapas:

1. **Descubrimiento (DHCP Discover)**: Cuando un dispositivo se conecta a la red y necesita una dirección IP, envía un mensaje de broadcast llamado **DHCP Discover** para buscar un servidor DHCP disponible.

2. **Oferta (DHCP Offer)**: El servidor DHCP responde al mensaje de broadcast con un mensaje **DHCP Offer**, que incluye una dirección IP disponible y la configuración de red, como la máscara de subred, la puerta de enlace (gateway) predeterminada, y los servidores DNS.

3. **Solicitud (DHCP Request)**: El cliente elige una de las ofertas recibidas y envía un mensaje **DHCP Request** al servidor, solicitando la dirección IP ofrecida y confirmando su intención de usarla.

4. **Confirmación (DHCP Acknowledgement o ACK)**: El servidor confirma la asignación enviando un mensaje **DHCP ACK**, que finaliza el proceso de configuración y asigna la IP al cliente.

Este proceso garantiza que cada dispositivo de la red reciba una dirección IP única y válida.

#### Parámetros Asignados por DHCP

Además de la dirección IP, el servidor DHCP puede asignar otros parámetros esenciales para la configuración de red, tales como:
- **Máscara de subred**: Define la porción de la IP que identifica la red y la porción que identifica al dispositivo en la red.
- **Puerta de enlace predeterminada (Gateway)**: IP del dispositivo que conecta la red local a otras redes.
- **Servidores DNS**: Direcciones IP de los servidores que resuelven nombres de dominio.
- **Tiempo de concesión (Lease Time)**: Tiempo durante el cual el cliente puede usar la dirección IP asignada.

#### Ejemplo Práctico de DHCP

Imagina una oficina en la que los empleados utilizan dispositivos (ordenadores, impresoras, teléfonos IP) que necesitan conectarse a la red. En lugar de asignar manualmente una dirección IP a cada dispositivo, la red cuenta con un servidor DHCP que asigna IPs automáticamente. 

1. **Configuración inicial**: El administrador configura el servidor DHCP para que gestione un rango de IPs, por ejemplo, desde `192.168.1.100` hasta `192.168.1.200`.

2. **Proceso de conexión**:
   - Un ordenador se conecta a la red por primera vez y envía un mensaje **DHCP Discover** en busca de un servidor DHCP.
   - El servidor DHCP responde con un **DHCP Offer** que le asigna una IP dentro del rango, digamos `192.168.1.101`.
   - El ordenador responde con **DHCP Request** para confirmar que quiere usar esa IP.
   - El servidor envía un **DHCP ACK** confirmando la asignación de la IP, junto con otros parámetros (puerta de enlace, máscara de subred, etc.).

3. **Renovación de IP**: Si el ordenador permanece conectado durante mucho tiempo, una vez que la concesión de la IP se esté por vencer, el dispositivo envía una solicitud de renovación al servidor DHCP para prolongar el uso de la misma dirección IP.

#### Ventajas del uso de DHCP

- **Automatización**: Reduce la carga administrativa, eliminando la necesidad de asignar manualmente direcciones IP.
- **Evita conflictos de IP**: Asegura que no haya direcciones IP duplicadas en la red.
- **Escalabilidad**: Es ideal para redes grandes o en expansión, donde la administración manual sería impracticable.

#### Ejemplo de Configuración de un Servidor DHCP en Linux

Para configurar un servidor DHCP en Linux, podríamos usar `isc-dhcp-server`, un servicio ampliamente utilizado. Aquí tienes un ejemplo de configuración básica:

1. Instala el servidor DHCP:

   ```bash
   sudo apt update
   sudo apt install isc-dhcp-server
   ```

2. Configura el rango de direcciones IP en el archivo `/etc/dhcp/dhcpd.conf`:

   ```plaintext
   subnet 192.168.1.0 netmask 255.255.255.0 {
       range 192.168.1.100 192.168.1.200;
       option routers 192.168.1.1;
       option subnet-mask 255.255.255.0;
       option domain-name-servers 8.8.8.8, 8.8.4.4;
       default-lease-time 600;
       max-lease-time 7200;
   }
   ```

3. Inicia y habilita el servicio DHCP:

   ```bash
   sudo systemctl start isc-dhcp-server
   sudo systemctl enable isc-dhcp-server
   ```

Con esta configuración, el servidor DHCP asignará direcciones IP desde `192.168.1.100` hasta `192.168.1.200`, con `192.168.1.1` como puerta de enlace predeterminada y servidores DNS de Google.
