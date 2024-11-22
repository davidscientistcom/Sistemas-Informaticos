### **Diseño de Redes para una Empresa Mediana**

Este ejercicio nos sitúa en una empresa mediana que necesita gestionar sus redes internas de manera eficiente. Vamos a diseñar las redes, calcular las subredes, justificar cada decisión y resolver posibles casos prácticos.

---

### **Planteamiento**

#### **Escenario**
- La empresa está en un edificio de tres plantas.
- Cada planta alberga varios departamentos:
  1. **Informática** (30 empleados).
  2. **Matemáticos** (10 empleados).
  3. **Administración** (5 empleados).
  4. **Gerencia** (2 empleados).
  5. **Sistemas locales** (8 servidores, para desarrollo y bases de datos).
- Requerimientos:
  - Cada departamento debe tener su red aislada (subred propia).
  - Los sistemas locales deben ser accesibles desde cualquier departamento.
  - Acceso SSH desde el exterior.
  - Un matemático necesita montar un servidor FTP para compartir modelos matemáticos con Informática.

---

### **1. Adquirir una Dirección IP Pública**

#### **¿Qué es una IP Pública?**
Una IP pública es una dirección única en Internet que permite que nuestra red interna sea accesible desde el exterior. Es imprescindible para servicios como SSH, hosting de aplicaciones o servidores FTP.

#### **Proceso de Adquisición**
1. **Contactar con un ISP**: Pedimos un plan empresarial con IP pública estática.
2. **Elegir el plan**:
   - IP pública estática (20-50 €/mes).
   - Ejemplo: Nos asignan la dirección `85.34.56.78` con la máscara `255.255.255.248`, que permite hasta 6 direcciones útiles.
3. **Configurar la IP Pública en el Router Principal**:
   - WAN (conexión a Internet): Se asigna la IP pública proporcionada por el ISP.
   - LAN (red interna): Se configurará para manejar las subredes internas.

---

### **2. Diseño de la Red Interna**

#### **Paso 1: Elección de la Red Privada**
Elegimos una dirección IP privada para nuestra red interna. Optamos por una red de clase C:
- Dirección: **192.168.0.0/24** (dirección privada común).
- Soporta hasta 254 hosts.

#### **Paso 2: Dividir la Red en Subredes**
Queremos dividir esta red en subredes para cada departamento y sistemas. Usaremos subnetting para lograrlo.

**Requisitos:**
- **Informática:** 30 empleados.
- **Matemáticos:** 10 empleados.
- **Administración:** 5 empleados.
- **Gerencia:** 2 empleados.
- **Sistemas:** 8 servidores.

**Cálculo de las subredes:**
1. **Tamaño mínimo por subred:**  
   La subred más grande necesita 30 hosts. Recordemos:
   - Cada subred debe tener:
     - **1 dirección de red** (identifica la subred).
     - **1 dirección de broadcast** (difusión dentro de la subred).
   - Fórmula para calcular hosts útiles: \( 2^n - 2 \), donde \( n \) es el número de bits para hosts.

2. **Número de bits necesarios para hosts:**  
   - Para 30 hosts útiles, necesitamos al menos \( 2^5 = 32 \) direcciones.  
     Esto implica 5 bits para la parte de hosts.

3. **Nueva máscara de subred:**  
   - La máscara original era /24 (`255.255.255.0`).  
   - Tomamos 3 bits adicionales para las subredes (total: 27 bits para red).
   - Nueva máscara: `/27` (`255.255.255.224`).

---

### **3. Cálculo Detallado de las Subredes**

Vamos a dividir la red `192.168.0.0/24` en bloques de `/27`.

#### **Subred 1: Informática**
1. **Dirección base:** `192.168.0.0`
2. **Máscara en binario:**  
   Máscara /27 = `11111111.11111111.11111111.11100000`  
3. **Cálculo de la dirección de red (AND bit a bit):**
   - Dirección IP: `11000000.10101000.00000000.00000000`
   - Máscara:      `11111111.11111111.11111111.11100000`
   - Resultado:    `11000000.10101000.00000000.00000000` = `192.168.0.0` (dirección de red).

4. **Dirección de broadcast:**
   - Poner todos los bits de host en `1`:  
     `11000000.10101000.00000000.00011111` = `192.168.0.31`.

5. **Rango de hosts útiles:**
   - Desde: `192.168.0.1`
   - Hasta: `192.168.0.30`.

#### **Subred 2: Matemáticos**
1. **Dirección base:** `192.168.0.32`
2. **Cálculo de la dirección de red:**
   - Dirección IP: `11000000.10101000.00000000.00100000`
   - Máscara:      `11111111.11111111.11111111.11100000`
   - Resultado:    `11000000.10101000.00000000.00100000` = `192.168.0.32`.

3. **Dirección de broadcast:**
   - `11000000.10101000.00000000.00111111` = `192.168.0.63`.

4. **Rango de hosts útiles:**
   - Desde: `192.168.0.33`
   - Hasta: `192.168.0.62`.

---

### **Resto de Subredes**

- **Administración:** `192.168.0.64/27`  
  Hosts: `192.168.0.65` - `192.168.0.94`.  
  Broadcast: `192.168.0.95`.

- **Gerencia:** `192.168.0.96/27`  
  Hosts: `192.168.0.97` - `192.168.0.126`.  
  Broadcast: `192.168.0.127`.

- **Sistemas:** `192.168.0.128/27`  
  Hosts: `192.168.0.129` - `192.168.0.158`.  
  Broadcast: `192.168.0.159`.

---

### **4. Caso del Servidor FTP**

#### Requerimiento
Un matemático necesita montar un servidor FTP para compartir modelos matemáticos con Informática.

#### Solución
El servidor FTP debe estar accesible para todos, pero centralizado para facilitar la gestión.

1. **Ubicación del servidor:**  
   - Subred de Sistemas: `192.168.0.140`.
2. **Configuración de acceso:**  
   - En el router principal, configurar reglas para permitir acceso al puerto FTP (21) desde todas las subredes internas.
3. **Ventaja de centralización:**  
   - Mayor seguridad y control.
   - Evitamos saturar otras subredes.

---
