## 1. NAT (Network Address Translation): Traducción de Direcciones IP

### 1.1 Concepto y Funcionamiento de NAT

**NAT** es una técnica que permite que varios dispositivos en una red privada utilicen una única dirección IP pública para acceder a redes externas, como Internet. Esto es particularmente útil en redes con
 direcciones IP privadas, que no son enrutable en Internet y necesitan ser traducidas a una dirección pública para comunicarse con dispositivos externos.

#### Propósito de NAT
- **Ahorro de Direcciones IP Públicas**: Permite que múltiples dispositivos usen una sola dirección IP pública, lo cual es especialmente útil dada la escasez de direcciones IPv4.
- **Seguridad**: Oculta las direcciones IP internas de la red privada, lo cual dificulta el acceso directo desde el exterior a dispositivos internos.
- **Flexibilidad**: Facilita la gestión de direcciones IP, permitiendo cambiar direcciones privadas sin afectar la dirección pública asignada.

#### Funcionamiento de NAT
Cuando un dispositivo en la red privada envía un paquete de datos a una red externa (por ejemplo, accediendo a un sitio web), NAT modifica la dirección IP de origen del paquete, sustituyendo la dirección IP privada del dispositivo por la dirección IP pública del router. Luego, NAT registra la conexión en una tabla de traducción que asocia la dirección IP privada y el puerto de origen con la dirección IP pública.

---

### 1.2 Tipos de NAT: Estático, Dinámico y PAT (Port Address Translation)

Existen varios tipos de NAT, cada uno con sus propias características y usos. A continuación, se explican los tres tipos principales:

#### 1. NAT Estático
- **Descripción**: Asigna de forma permanente una dirección IP pública a una IP privada específica. Esto significa que una dirección IP privada específica siempre se traduce a la misma dirección IP pública.
- **Uso Típico**: Utilizado cuando un dispositivo interno, como un servidor web, debe ser accesible desde el exterior siempre con la misma dirección IP pública.
  
  **Ejemplo**:
  - Dirección IP privada del servidor interno: `192.168.1.10`
  - Dirección IP pública asignada a este servidor: `203.0.113.5`
  
  Con NAT estático, cualquier paquete enviado a `203.0.113.5` será redirigido a `192.168.1.10` en la red privada.

#### 2. NAT Dinámico
- **Descripción**: Asigna direcciones IP públicas a las IPs privadas de manera dinámica, utilizando un conjunto de direcciones IP públicas. La dirección IP pública asignada puede variar en cada conexión.
- **Uso Típico**: Usado en redes grandes donde hay un conjunto de direcciones IP públicas disponibles para un grupo de dispositivos, pero no necesariamente para todos a la vez.

  **Ejemplo**:
  - Rango de IP privadas: `192.168.1.0/24`
  - Rango de IP públicas asignadas para NAT: `203.0.113.5 - 203.0.113.10`
  
  Cuando un dispositivo de la red `192.168.1.0/24` envía tráfico hacia Internet, se le asigna temporalmente una de las IPs públicas en el rango `203.0.113.5 - 203.0.113.10`.

#### 3. PAT (Port Address Translation) o NAT con Sobrecarga
- **Descripción**: Permite que varios dispositivos en una red privada compartan una sola dirección IP pública. Se diferencia de los otros tipos de NAT porque utiliza números de puerto para distinguir entre las conexiones de diferentes dispositivos.
- **Uso Típico**: Es el tipo de NAT más utilizado en redes domésticas y en pequeñas empresas, ya que permite que muchos dispositivos compartan una única dirección IP pública.

  **Ejemplo**:
  - Dirección IP privada de un PC: `192.168.1.20`
  - Dirección IP pública compartida para todos los dispositivos: `203.0.113.5`
  
  El router utilizará diferentes números de puerto para cada dispositivo que envíe tráfico a Internet. Por ejemplo:
    - `192.168.1.20:12345` se traduce a `203.0.113.5:1024`
    - `192.168.1.21:12346` se traduce a `203.0.113.5:1025`
  
  De esta forma, varios dispositivos pueden usar la misma IP pública, diferenciándose por el número de puerto.

---

### 1.3 Configuración de NAT en Redes Privadas y Públicas

Para aplicar NAT en un router o firewall, es necesario configurar las direcciones IP privadas y públicas que NAT traducirá, así como especificar el tipo de NAT a utilizar. A continuación, se muestran ejemplos detallados de configuración para cada tipo de NAT.

---

### 1.4 Ejemplos Prácticos de Configuración de NAT

#### Ejemplo 1: Configuración de NAT Estático para un Servidor Web Interno

**Escenario**:
- Una empresa tiene un servidor web en la red privada (`192.168.10.10`) que necesita ser accesible desde Internet.
- La empresa cuenta con una dirección IP pública (`203.0.113.5`) que será asignada exclusivamente al servidor web usando NAT estático.

**Configuración en el Router**:
1. Configuramos una entrada de NAT estático que asocia la IP privada con la IP pública.
2. La entrada asegura que el tráfico entrante a `203.0.113.5` se redirija al servidor en `192.168.10.10`.

**Comandos de Configuración**:
```plaintext
ip nat inside source static 192.168.10.10 203.0.113.5
interface FastEthernet0/0
 ip address 192.168.10.1 255.255.255.0
 ip nat inside
interface Serial0/0
 ip address 203.0.113.5 255.255.255.248
 ip nat outside
```

En este ejemplo:
- La interfaz `FastEthernet0/0` está conectada a la red interna y configurada como `ip nat inside`.
- La interfaz `Serial0/0` tiene la IP pública y se configura como `ip nat outside`.

---

#### Ejemplo 2: Configuración de NAT Dinámico para una Red con IPs Privadas

**Escenario**:
- Una red privada (`192.168.20.0/24`) con múltiples dispositivos necesita acceso a Internet.
- La empresa tiene un grupo de direcciones IP públicas (`203.0.113.5 - 203.0.113.10`) disponibles para asignación dinámica.

**Configuración en el Router**:
1. Definimos un pool (grupo) de direcciones IP públicas para NAT dinámico.
2. Configuramos la NAT en el router para que asigne una de estas direcciones IP públicas a cualquier dispositivo que necesite acceder a Internet.

**Comandos de Configuración**:
```plaintext
ip nat pool mypool 203.0.113.5 203.0.113.10 netmask 255.255.255.248
access-list 1 permit 192.168.20.0 0.0.0.255
ip nat inside source list 1 pool mypool
interface FastEthernet0/1
 ip address 192.168.20.1 255.255.255.0
 ip nat inside
interface Serial0/0
 ip address 203.0.113.1 255.255.255.248
 ip nat outside
```

En este ejemplo:
- El pool `mypool` define el rango de direcciones IP públicas disponibles para NAT dinámico.
- La lista de acceso `1` permite que la red `192.168.20.0/24` utilice NAT.
- Las interfaces se configuran como `ip nat inside` para la red interna y `ip nat outside` para la conexión a Internet.

---

#### Ejemplo 3: Configuración de PAT para Red Hogareña con Una IP Pública

**Escenario**:
- Una red doméstica con varios dispositivos conectados a través de una dirección IP privada (`192.168.30.0/24`) necesita acceder a Internet.
- La conexión a Internet utiliza una sola dirección IP pública (`203.0.113.5`).

**Configuración en el Router**:
1. Configuramos PAT para que todos los dispositivos en `192.168.30.0/24` compartan la IP pública `203.0.113.5`.
2. Utilizamos puertos para diferenciar las conexiones de cada dispositivo.

**Comandos de Configuración**:
```plaintext
access-list 1 permit 192.168.30.0 0.0.0.255
ip nat inside source list 1 interface Serial0/0 overload
interface FastEthernet0/1
 ip address 192.168.30.1 255.255.255.0
 ip nat inside
interface Serial0/0
 ip address 203.0.113.5 255.255.255.248
 ip nat outside
```

En este ejemplo:
- La lista de acceso `1

` permite que toda la red `192.168.30.0/24` utilice NAT.
- El comando `overload` en `ip nat inside source list 1 interface Serial0/0 overload` habilita PAT, permitiendo que múltiples dispositivos compartan la misma IP pública.

---

### Resumen del Concepto y Ejemplos de NAT

**Tipos de NAT y Aplicaciones**:
- **NAT Estático**: Para dispositivos internos que necesitan una IP pública fija (ej. servidores).
- **NAT Dinámico**: Para redes privadas grandes con un rango de IPs públicas compartidas.
- **PAT**: Para redes pequeñas (como redes domésticas) donde múltiples dispositivos comparten una única IP pública.
