### **Explicación del Funcionamiento de Redes en Subnetting (MAC e IP)**

Vamos a analizar cómo funciona el proceso completo desde el nivel de enlace (MAC) hasta el nivel de red (IP), considerando un escenario en el que:

1. **Se recibe una dirección IP inicial**, como `192.168.1.0/24`.
2. La dividimos en **4 subredes**.
3. Una de esas subredes se subdivide aún más.
4. Analizamos la comunicación dentro de las subredes y entre ellas.
5. Finalmente, revisamos cómo se conecta a Internet.



### **1. Funcionamiento a Nivel de Enlace (MAC)**

#### **¿Qué es la dirección MAC?**
- La dirección MAC es un identificador único asignado a la tarjeta de red de un dispositivo. Se utiliza para identificar dispositivos en una red local (LAN).
- Es independiente de la dirección IP.
- Formato: **6 pares de dígitos hexadecimales**, por ejemplo, `00:1A:2B:3C:4D:5E`.

#### **¿Cómo trabaja el nivel de enlace?**
1. Dentro de una subred, los dispositivos **no usan direcciones IP para enviar datos directamente**. En cambio, usan direcciones MAC.
2. El protocolo **ARP (Address Resolution Protocol)** se encarga de traducir una dirección IP a una dirección MAC.

#### **Ejemplo: Comunicación dentro de una subred**
- IP de origen: `192.168.1.2` (MAC: `00:1A:2B:3C:4D:01`).
- IP de destino: `192.168.1.3` (MAC: `00:1A:2B:3C:4D:02`).

1. El dispositivo `192.168.1.2` consulta su tabla ARP para encontrar la MAC asociada a `192.168.1.3`.
   - Si no la encuentra, envía una **solicitud ARP**:  
     "¿Quién tiene `192.168.1.3`? Dime tu MAC."
   - `192.168.1.3` responde con su dirección MAC.

2. Una vez que `192.168.1.2` conoce la MAC, encapsula el paquete en un marco Ethernet:
   ```
   Cabecera Ethernet: 
   - Dirección MAC destino: 00:1A:2B:3C:4D:02
   - Dirección MAC origen: 00:1A:2B:3C:4D:01
   - Tipo: IP
   ```
3. El switch de la red local usa la dirección MAC para enviar el paquete al puerto correcto.



### **2. Funcionamiento a Nivel de Red (IP)**

El nivel de red trabaja con direcciones IP para identificar redes y dispositivos. Veamos cómo opera en el caso de subredes.

#### **a) Comunicación dentro de la misma subred**
- Si dos dispositivos están en la misma subred, no necesitan pasar por un router.
- El proceso es:
  1. El dispositivo fuente verifica si el destino está en la misma subred.
  2. Si lo está, consulta ARP, obtiene la MAC y envía el paquete directamente.

**Ejemplo:**
- Red: `192.168.1.0/28` (subred).
- Dispositivo 1: `192.168.1.2` (MAC: `00:1A:2B:3C:4D:01`).
- Dispositivo 2: `192.168.1.14` (MAC: `00:1A:2B:3C:4D:02`).

Ambos están en la misma subred, por lo que se comunican directamente, sin usar un router.



#### **b) Comunicación entre subredes**
Cuando dos dispositivos están en **diferentes subredes**, necesitan un router para comunicarse.

**Proceso:**
1. El dispositivo fuente envía el paquete al router.
   - Encapsula el paquete con la **dirección MAC del router** como destino.
   - Dirección IP destino: La IP del dispositivo en otra subred.

2. El router:
   - Recibe el paquete y consulta su **tabla de enrutamiento**.
   - Si tiene una ruta hacia la subred destino, reenvía el paquete por la interfaz correcta.
   - Antes de reenviar, el router:
     - Cambia la **MAC destino** por la del dispositivo final (usando ARP).
     - No cambia las direcciones IP.

**Ejemplo:**
- Subred 1: `192.168.1.0/28`.
  - Dispositivo: `192.168.1.2` (MAC: `00:1A:2B:3C:4D:01`).
- Subred 2: `192.168.1.16/28`.
  - Dispositivo: `192.168.1.18` (MAC: `00:1A:2B:3C:4D:03`).

1. `192.168.1.2` envía el paquete al router (MAC destino: MAC del router).
2. El router reenvía el paquete a `192.168.1.18` (cambia la MAC destino por `00:1A:2B:3C:4D:03`).



#### **c) Salida a Internet**
Cuando un dispositivo necesita comunicarse con una dirección fuera de la red local (por ejemplo, `8.8.8.8`), el proceso es:

1. **Reenvío al router**:
   - El dispositivo envía el paquete al router, encapsulándolo con la MAC del router como destino.

2. **Traducción de direcciones IP (NAT)**:
   - El router traduce la dirección IP privada del dispositivo (por ejemplo, `192.168.1.2`) a su dirección IP pública (por ejemplo, `85.34.56.78`).
   - Esto permite que el paquete viaje por Internet.

3. **Encaminamiento en Internet**:
   - El router envía el paquete a la siguiente puerta de enlace (del proveedor de Internet).
   - En Internet, los routers usan tablas de enrutamiento globales para encontrar la mejor ruta hacia el destino.

4. **Respuesta desde Internet**:
   - El paquete de respuesta llega al router con la dirección IP pública como destino.
   - El router usa su tabla NAT para traducir la IP pública de vuelta a la IP privada del dispositivo original.



### **3. Ejemplo Completo: Comunicación entre Subredes y Salida a Internet**

Supongamos que un dispositivo en la Subred 1 necesita comunicarse con un servidor en Internet (`8.8.8.8`):

#### **Redes configuradas:**
- **Subred 1:** `192.168.1.0/28` (Router: `192.168.1.1`).
- **Subred 2:** `192.168.1.16/28` (Router: `192.168.1.17`).
- **IP Pública del Router:** `85.34.56.78`.

#### **Paso a Paso:**
1. **El dispositivo fuente envía el paquete al router**:
   - IP destino: `8.8.8.8`.
   - MAC destino: MAC del router (`00:AA:BB:CC:DD:01`).

2. **El router realiza NAT**:
   - Traduce la IP privada del dispositivo (`192.168.1.2`) a su IP pública (`85.34.56.78`).
   - Reenvía el paquete a la puerta de enlace del ISP.

3. **El ISP enruta el paquete hacia `8.8.8.8`**:
   - Usa su tabla de enrutamiento global para encontrar la mejor ruta.

4. **El servidor responde**:
   - IP destino: `85.34.56.78`.
   - El paquete llega al router.

5. **El router deshace la traducción NAT**:
   - Traduce la IP pública (`85.34.56.78`) a la IP privada original (`192.168.1.2`).
   - Encapsula el paquete con la MAC del dispositivo (`00:1A:2B:3C:4D:01`).

6. **El paquete llega al dispositivo fuente**.
--- 
---
### COMUNICACIÓN ENTRE SUBREDES
---
---

### **1. Contexto: Comunicación entre Subredes**

Cuando hacemos subnetting, dividimos una red más grande en varias subredes más pequeñas. Estas subredes quedan **aisladas entre sí**, ya que están en **diferentes rangos de direcciones IP**. Esto implica que **no pueden comunicarse directamente sin un dispositivo que entienda cómo enrutar el tráfico entre ellas**, como un **router**.

#### **Regla básica:**
- Si los dispositivos están en la **misma subred**, se comunican directamente usando sus direcciones MAC (nivel de enlace).
- Si están en **diferentes subredes**, **siempre necesitan un router** para comunicarse.


### **2. Escenario: Subred de la Subred**

Supongamos el siguiente caso:
1. La red base es `192.168.1.0/24`.
2. Hacemos subnetting inicial en 4 subredes:
   - **Subred A**: `192.168.1.0/26`.
   - **Subred B**: `192.168.1.64/26`.
   - **Subred C**: `192.168.1.128/26`.
   - **Subred D**: `192.168.1.192/26`.

Luego, subdividimos la **Subred A (`192.168.1.0/26`)** en otras 4 subredes más pequeñas:
- **Subred A1**: `192.168.1.0/28`.
- **Subred A2**: `192.168.1.16/28`.
- **Subred A3**: `192.168.1.32/28`.
- **Subred A4**: `192.168.1.48/28`.


### **3. Comunicación entre Subredes**

#### **Caso 1: Comunicación entre dos subredes hijas (A1 y A2)**
- **¿Están en el mismo rango de IPs?**  
  No, A1 es `192.168.1.0/28` y A2 es `192.168.1.16/28`. Cada subred tiene un rango de direcciones único.
  
- **¿Se necesita un router?**  
  Sí. Aunque ambas subredes pertenecen al "bloque padre" (`192.168.1.0/26`), están divididas y no pueden comunicarse directamente.

**Ejemplo práctico:**
- Dispositivo en A1 (`192.168.1.2`) quiere comunicarse con un dispositivo en A2 (`192.168.1.18`):
  - El paquete se envía al **router**, ya que no están en la misma subred.
  - El router analiza su tabla de enrutamiento:
    - Ve que A1 está conectada a una interfaz.
    - Ve que A2 está conectada a otra interfaz.
  - El router reenvía el paquete a A2.

#### **Caso 2: Comunicación entre subred hija (A1) y subred hermana (B)**
- **¿Están en el mismo rango de IPs?**  
  No, A1 es `192.168.1.0/28` y B es `192.168.1.64/26`. Estas subredes están completamente separadas.

- **¿Se necesita un router?**  
  Sí. La comunicación entre subredes hermanas siempre pasa por un router.



### **4. ¿Por qué siempre se necesita un router?**

El motivo por el cual necesitas un router entre subredes es que **las direcciones IP de las subredes son diferentes**. Un dispositivo en una subred no tiene manera de enviar directamente un paquete a otra subred porque:
1. Los switches trabajan en la **Capa 2 (nivel de enlace)** y solo entienden direcciones MAC.
2. Los routers trabajan en la **Capa 3 (nivel de red)** y entienden direcciones IP.

**¿Qué hace el router?**
- El router actúa como un intermediario entre subredes. Consulta su **tabla de enrutamiento** para decidir a qué subred enviar el paquete.



### **5. Comunicación con la "Subred Padre"**

#### **¿Qué es la subred padre en este caso?**
La **subred padre** es la red original antes del segundo nivel de subnetting, como `192.168.1.0/26`.

#### **¿Se necesita un router para comunicarse con la subred padre?**
Sí, porque desde la perspectiva del direccionamiento IP:
1. Las subredes hijas (por ejemplo, A1 y A2) tienen máscaras más largas que las de la subred padre (`/28` vs `/26`).
2. Esto las hace "distintas" desde el punto de vista de las direcciones IP.

**Ejemplo:**
- Un dispositivo en la subred A1 (`192.168.1.2`) quiere comunicarse con un dispositivo en la subred padre (`192.168.1.50` en A):
  - El paquete se envía al router porque están en rangos de direcciones diferentes.
  - El router consulta su tabla de enrutamiento y reenvía el paquete.



### **6. Comunicación con Internet**

Si un dispositivo en una subred quiere comunicarse con Internet, también necesita pasar por un router. El proceso es el siguiente:

1. **El dispositivo envía el paquete al router**:
   - Dirección IP destino: La IP del servidor en Internet (por ejemplo, `8.8.8.8`).
   - Dirección MAC destino: La MAC del router.

2. **El router traduce la dirección IP (NAT)**:
   - Convierte la dirección IP privada del dispositivo (por ejemplo, `192.168.1.2`) en su dirección IP pública (por ejemplo, `85.34.56.78`).

3. **El router envía el paquete hacia Internet**:
   - Usa su interfaz conectada al proveedor de Internet.

4. **El paquete regresa desde Internet**:
   - La respuesta llega al router con la IP pública (`85.34.56.78`).
   - El router usa su tabla NAT para traducir la dirección IP pública de vuelta a la IP privada del dispositivo.



### **7. Conclusión**

- **Siempre se necesita un router para comunicar subredes diferentes**, incluso si una subred es hija de otra.
- Los routers trabajan en el nivel de red (IP) y son responsables de decidir por dónde enviar el tráfico entre subredes.
- Para comunicarse con Internet, también es necesario un router con NAT para traducir entre direcciones privadas y públicas.
