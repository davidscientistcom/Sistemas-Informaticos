## 2. Segmentación de Redes con VLAN (Virtual Local Area Network)

### 2.1 Concepto y Función de las VLANs

Una **VLAN** (Red de Área Local Virtual) es una red lógica creada dentro de un switch o conjunto de switches. Las VLAN permiten segmentar el tráfico en una red física en múltiples redes virtuales independientes, lo cual mejora la seguridad y el rendimiento. Cada VLAN actúa como una subred separada dentro de la red física, lo que significa que los dispositivos en una VLAN no pueden comunicarse directamente con dispositivos en otra VLAN sin un dispositivo de ruteo (como un router o un switch de capa 3).

#### Ventajas de Utilizar VLANs
- **Seguridad**: Las VLANs limitan el tráfico de broadcast y separan los dispositivos, permitiendo un control más preciso de qué dispositivos pueden comunicarse entre sí.
- **Organización**: Facilitan la creación de redes independientes dentro de la misma infraestructura física, ideal para entornos con múltiples departamentos.
- **Eficiencia**: Reducen el tráfico no deseado dentro de cada VLAN, mejorando el rendimiento de la red.
- **Flexibilidad**: Los dispositivos pueden moverse físicamente sin cambiar la configuración de red, ya que se pueden reconfigurar las VLANs de manera lógica.

---

### 2.2 Tipos de VLANs: VLAN por Puerto, VLAN por Protocolo y VLAN por MAC

Existen varios tipos de VLANs, cada una adaptada a necesidades específicas. A continuación, se explican los tres tipos principales:

#### 1. VLAN por Puerto (Port-based VLAN)
- **Descripción**: Asigna cada puerto del switch a una VLAN específica. Los dispositivos conectados a ese puerto pertenecen a la VLAN correspondiente.
- **Uso Típico**: Es el tipo de VLAN más común y sencillo de configurar. Se utiliza para separar redes por departamentos o áreas específicas.
  
  **Ejemplo**:
  - En un switch, los puertos `1-10` pueden asignarse a la VLAN de administración (VLAN 10) y los puertos `11-20` a la VLAN de ventas (VLAN 20).

#### 2. VLAN por Protocolo (Protocol-based VLAN)
- **Descripción**: Asigna dispositivos a una VLAN basada en el tipo de protocolo de tráfico (como IPv4 o IPv6).
- **Uso Típico**: Menos común, se utiliza en redes complejas donde ciertos protocolos necesitan ser aislados para mejorar el rendimiento o la seguridad.

  **Ejemplo**:
  - En una red que maneja tanto tráfico IPv4 como IPv6, los paquetes IPv4 pueden asignarse a VLAN 30 y los paquetes IPv6 a VLAN 40.

#### 3. VLAN por Dirección MAC (MAC-based VLAN)
- **Descripción**: Asigna dispositivos a una VLAN en función de su dirección MAC. La dirección MAC de cada dispositivo determina a qué VLAN pertenece.
- **Uso Típico**: Se usa en redes donde los dispositivos pueden moverse físicamente pero deben permanecer en la misma VLAN, independientemente del puerto al que estén conectados.

  **Ejemplo**:
  - La dirección MAC de una impresora puede asignarse a una VLAN específica, de modo que la impresora permanezca en la misma VLAN incluso si se mueve de puerto en el switch.

---

### 2.3 Configuración de VLANs en Switches Administrables

La configuración de VLANs se realiza en switches administrables que soportan la creación de VLANs y el ruteo entre ellas. A continuación, se muestra una configuración básica para segmentar una red utilizando VLANs.

#### Escenario de Ejemplo

Supongamos que tenemos una empresa con tres departamentos: Administración, Ventas y Soporte Técnico. La empresa quiere separar el tráfico de cada departamento en diferentes VLANs para mejorar la seguridad y organización de la red.

1. **VLAN 10** - Administración
2. **VLAN 20** - Ventas
3. **VLAN 30** - Soporte Técnico

---

#### Paso 1: Crear VLANs en el Switch

El primer paso es crear las VLANs en el switch y asignarles un número de VLAN y un nombre que identifique cada departamento.

**Comandos de Configuración en el Switch**:
```plaintext
Switch# configure terminal
Switch(config)# vlan 10
Switch(config-vlan)# name Administracion
Switch(config-vlan)# exit

Switch(config)# vlan 20
Switch(config-vlan)# name Ventas
Switch(config-vlan)# exit

Switch(config)# vlan 30
Switch(config-vlan)# name SoporteTecnico
Switch(config-vlan)# exit
```

#### Paso 2: Asignar Puertos a Cada VLAN

Una vez creadas las VLANs, el siguiente paso es asignar los puertos a las VLAN correspondientes. Esto asegura que los dispositivos conectados a cada puerto pertenezcan a la VLAN adecuada.

**Ejemplo de Asignación de Puertos**:
- **Puertos 1-10** para Administración (VLAN 10)
- **Puertos 11-20** para Ventas (VLAN 20)
- **Puertos 21-30** para Soporte Técnico (VLAN 30)

**Comandos de Configuración en el Switch**:
```plaintext
Switch(config)# interface range FastEthernet 0/1 - 10
Switch(config-if-range)# switchport mode access
Switch(config-if-range)# switchport access vlan 10
Switch(config-if-range)# exit

Switch(config)# interface range FastEthernet 0/11 - 20
Switch(config-if-range)# switchport mode access
Switch(config-if-range)# switchport access vlan 20
Switch(config-if-range)# exit

Switch(config)# interface range FastEthernet 0/21 - 30
Switch(config-if-range)# switchport mode access
Switch(config-if-range)# switchport access vlan 30
Switch(config-if-range)# exit
```

En esta configuración:
- Los puertos `0/1 - 10` pertenecen a la VLAN 10 (Administración).
- Los puertos `0/11 - 20` pertenecen a la VLAN 20 (Ventas).
- Los puertos `0/21 - 30` pertenecen a la VLAN 30 (Soporte Técnico).

---

### 2.4 Ejemplos Prácticos de Segmentación con VLANs

#### Ejemplo 1: Red de Oficina con Tres VLANs

**Escenario**:
- Una oficina tiene tres departamentos: Administración, Ventas y Soporte Técnico, cada uno con su propia VLAN.
- Se requiere que cada VLAN esté aislada, pero que puedan comunicarse con un servidor compartido en una VLAN especial de servidores (VLAN 99).

1. **Crear y Configurar VLANs para Cada Departamento**.
2. **Crear VLAN para el Servidor (VLAN 99)**.
3. **Configurar un router o switch de capa 3** para rutar el tráfico entre las VLANs cuando sea necesario.

**Configuración del Servidor en VLAN 99**:
```plaintext
Switch(config)# vlan 99
Switch(config-vlan)# name Servidores
Switch(config-vlan)# exit
Switch(config)# interface FastEthernet 0/31
Switch(config-if)# switchport mode access
Switch(config-if)# switchport access vlan 99
Switch(config-if)# exit
```

En este ejemplo, el servidor en VLAN 99 es accesible desde las VLAN de los departamentos solo a través de un router o un switch de capa 3 que permita ruteo inter-VLAN.

---

#### Ejemplo 2: Configuración de Enlace Troncal (Trunk) entre Dos Switches

Si hay varios switches en la red y necesitamos que las VLANs se extiendan entre ellos, debemos configurar un enlace troncal. El enlace troncal permite que el tráfico de todas las VLANs se transmita entre los switches usando un solo enlace físico.

1. **Configurar el enlace troncal en los puertos de conexión entre los switches**.
2. **Permitir que el tráfico de múltiples VLANs viaje a través del enlace troncal**.

**Comandos para Configurar un Enlace Troncal**:
```plaintext
Switch1(config)# interface FastEthernet 0/24
Switch1(config-if)# switchport mode trunk
Switch1(config-if)# switchport trunk allowed vlan 10,20,30,99
Switch1(config-if)# exit

Switch2(config)# interface FastEthernet 0/24
Switch2(config-if)# switchport mode trunk
Switch2(config-if)# switchport trunk allowed vlan 10,20,30,99
Switch2(config-if)# exit
```

En este caso:
- Configuramos el puerto `FastEthernet 0/24` en ambos switches como enlace troncal.
- Permitimos el tráfico de las VLANs `10`, `20`, `30`, y `99` en el troncal.

**Nota**: Cuando el tráfico de VLAN viaja a través de un enlace troncal, se etiqueta con un identificador de VLAN para que los switches sepan a qué VLAN pertenece cada paquete.

---

### Resumen del Concepto y Ejemplos de VLANs

**Tipos de VLAN y Aplicaciones**:
- **VLAN por Puerto**: Asigna una VLAN específica a un puerto del switch, útil para segmentación básica.
- **VLAN por Protocolo**:

 Utiliza el tipo de protocolo (como IPv4 o IPv6) para segmentar el tráfico.
- **VLAN por MAC**: Asigna dispositivos a una VLAN en función de su dirección MAC, útil cuando los dispositivos pueden moverse físicamente.
