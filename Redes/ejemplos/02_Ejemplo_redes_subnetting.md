### Ejercicio: Subnetting de `192.168.1.0/24` en 4 subredes

**Datos del ejercicio:**
- **Red original:** 192.168.1.0/24
- **Número de subredes deseadas:** 4

#### Paso 1: Entender la máscara original

La red original es `192.168.1.0/24`, con una máscara de subred de `255.255.255.0`. En binario, esta máscara es:

```
11111111.11111111.11111111.00000000
```

Esta notación `/24` significa que los primeros 24 bits son "1" y los últimos 8 bits son "0", lo cual permite que tengamos una sola red de clase C con 254 hosts posibles (2^8 - 2).

#### Paso 2: Determinar la cantidad de bits adicionales necesarios para las subredes

Queremos dividir esta red en 4 subredes. Para lograrlo, necesitamos 2 bits adicionales, ya que 2^2 = 4. Esto implica "pedir prestados" 2 bits de la parte de host, convirtiendo dos de los ceros en unos en la máscara de subred.

#### Paso 3: Modificar la máscara de subred en binario

Modificamos la máscara original de `/24` a `/26`, cambiando dos ceros a unos. La nueva máscara en binario es:

```
11111111.11111111.11111111.11000000
```

En decimal, esta nueva máscara es `255.255.255.192`.

#### Paso 4: Determinar las subredes generadas

Con una máscara de `/26`, tenemos 4 subredes posibles y 64 direcciones en cada subred (2^6 = 64). En cada subred, 2 direcciones son reservadas para la dirección de red y la dirección de broadcast, por lo que quedan 62 direcciones de host en cada subred.

#### Paso 5: Calcular las direcciones de red, los rangos de IP y las direcciones de broadcast

Con la nueva máscara de `255.255.255.192`, tenemos los siguientes rangos:

1. **Subred 1:**
   - Dirección de red: `192.168.1.0`  
     Binario: `11000000.10101000.00000001.00000000`
   - Primer host: `192.168.1.1`
   - Último host: `192.168.1.62`
   - Dirección de broadcast: `192.168.1.63`  
     Binario: `11000000.10101000.00000001.00111111`

2. **Subred 2:**
   - Dirección de red: `192.168.1.64`  
     Binario: `11000000.10101000.00000001.01000000`
   - Primer host: `192.168.1.65`
   - Último host: `192.168.1.126`
   - Dirección de broadcast: `192.168.1.127`  
     Binario: `11000000.10101000.00000001.01111111`

3. **Subred 3:**
   - Dirección de red: `192.168.1.128`  
     Binario: `11000000.10101000.00000001.10000000`
   - Primer host: `192.168.1.129`
   - Último host: `192.168.1.190`
   - Dirección de broadcast: `192.168.1.191`  
     Binario: `11000000.10101000.00000001.10111111`

4. **Subred 4:**
   - Dirección de red: `192.168.1.192`  
     Binario: `11000000.10101000.00000001.11000000`
   - Primer host: `192.168.1.193`
   - Último host: `192.168.1.254`
   - Dirección de broadcast: `192.168.1.255`  
     Binario: `11000000.10101000.00000001.11111111`


### Ejercicio 1: Crear 4 subredes en la red `192.168.1.0/24`

**Datos del ejercicio:**
- **Red original:** 192.168.1.0/24 (máscara de clase C)
- **Subredes requeridas:** 4

**Paso 1:** Determinar la nueva máscara de subred

Dado que necesitamos 4 subredes, necesitamos 2 bits para representar las subredes (2^2 = 4 subredes). Esto implica una máscara de subred de /26:

- Máscara original: /24 = `255.255.255.0`
- Máscara nueva: /26 = `255.255.255.192`

**Paso 2:** Calcular el tamaño de cada subred

Con una máscara de /26, cada subred tiene 64 direcciones IP, de las cuales 2 son reservadas (dirección de red y broadcast), lo que deja 62 direcciones para hosts.

**Subredes generadas:**

| Subred | Dirección de Red | Primer Host    | Último Host    | Dirección de Broadcast |
|--------|-------------------|----------------|----------------|-------------------------|
| Subred 1 | 192.168.1.0    | 192.168.1.1    | 192.168.1.62   | 192.168.1.63            |
| Subred 2 | 192.168.1.64   | 192.168.1.65   | 192.168.1.126  | 192.168.1.127           |
| Subred 3 | 192.168.1.128  | 192.168.1.129  | 192.168.1.190  | 192.168.1.191           |
| Subred 4 | 192.168.1.192  | 192.168.1.193  | 192.168.1.254  | 192.168.1.255           |

---

### Ejercicio 2: Crear 8 subredes en la red `192.168.10.0/24`

**Datos del ejercicio:**
- **Red original:** 192.168.10.0/24
- **Subredes requeridas:** 8

**Paso 1:** Determinar la nueva máscara de subred

Para 8 subredes, necesitamos 3 bits adicionales (2^3 = 8), lo que resulta en una máscara de /27.

- Máscara original: /24 = `255.255.255.0`
- Máscara nueva: /27 = `255.255.255.224`

**Paso 2:** Calcular el tamaño de cada subred

Con una máscara de /27, cada subred tendrá 32 direcciones, de las cuales 30 son útiles para hosts.

**Subredes generadas:**

| Subred | Dirección de Red | Primer Host     | Último Host     | Dirección de Broadcast |
|--------|-------------------|-----------------|-----------------|-------------------------|
| Subred 1 | 192.168.10.0    | 192.168.10.1    | 192.168.10.30   | 192.168.10.31           |
| Subred 2 | 192.168.10.32   | 192.168.10.33   | 192.168.10.62   | 192.168.10.63           |
| Subred 3 | 192.168.10.64   | 192.168.10.65   | 192.168.10.94   | 192.168.10.95           |
| Subred 4 | 192.168.10.96   | 192.168.10.97   | 192.168.10.126  | 192.168.10.127          |
| Subred 5 | 192.168.10.128  | 192.168.10.129  | 192.168.10.158  | 192.168.10.159          |
| Subred 6 | 192.168.10.160  | 192.168.10.161  | 192.168.10.190  | 192.168.10.191          |
| Subred 7 | 192.168.10.192  | 192.168.10.193  | 192.168.10.222  | 192.168.10.223          |
| Subred 8 | 192.168.10.224  | 192.168.10.225  | 192.168.10.254  | 192.168.10.255          |

---

### Ejercicio 3: Crear 2 subredes en la red `192.168.5.0/24` para 126 hosts cada una

**Datos del ejercicio:**
- **Red original:** 192.168.5.0/24
- **Hosts requeridos por subred:** 126

**Paso 1:** Determinar la nueva máscara de subred

Para cada subred con 126 hosts, necesitamos al menos 7 bits (2^7 - 2 = 126 hosts), lo que implica una máscara de /25.

- Máscara original: /24 = `255.255.255.0`
- Máscara nueva: /25 = `255.255.255.128`

**Paso 2:** Calcular el tamaño de cada subred

Con una máscara de /25, cada subred contiene 128 direcciones, de las cuales 126 son utilizables para hosts.

**Subredes generadas:**

| Subred | Dirección de Red | Primer Host    | Último Host    | Dirección de Broadcast |
|--------|-------------------|----------------|----------------|-------------------------|
| Subred 1 | 192.168.5.0    | 192.168.5.1    | 192.168.5.126  | 192.168.5.127           |
| Subred 2 | 192.168.5.128  | 192.168.5.129  | 192.168.5.254  | 192.168.5.255           |

---

### Ejercicio 4: Crear 16 subredes en la red `192.168.15.0/24`

**Datos del ejercicio:**
- **Red original:** 192.168.15.0/24
- **Subredes requeridas:** 16

**Paso 1:** Determinar la nueva máscara de subred

Para crear 16 subredes, necesitamos 4 bits adicionales (2^4 = 16), lo que resulta en una máscara de /28.

- Máscara original: /24 = `255.255.255.0`
- Máscara nueva: /28 = `255.255.255.240`

**Paso 2:** Calcular el tamaño de cada subred

Con una máscara de /28, cada subred contiene 16 direcciones, de las cuales 14 son utilizables para hosts.

**Subredes generadas:**

| Subred | Dirección de Red | Primer Host     | Último Host     | Dirección de Broadcast |
|--------|-------------------|-----------------|-----------------|-------------------------|
| Subred 1 | 192.168.15.0    | 192.168.15.1    | 192.168.15.14   | 192.168.15.15           |
| Subred 2 | 192.168.15.16   | 192.168.15.17   | 192.168.15.30   | 192.168.15.31           |
| Subred 3 | 192.168.15.32   | 192.168.15.33   | 192.168.15.46   | 192.168.15.47           |
| Subred 4 | 192.168.15.48   | 192.168.15.49   | 192.168.15.62   | 192.168.15.63           |
| Subred 5 | 192.168.15.64   | 192.168.15.65   | 192.168.15.78   | 192.168.15.79           |
| Subred 6 | 192.168.15.80   | 192.168.15.81   | 192.168.15.94   | 192.168.15.95           |
| Subred 7 | 192.168.15.96   | 192.168.15.97   | 192.168.15.110  | 192.168.15.111          |
| Subred 8 | 192.168.15.112  | 192.168.15.113  | 192.168.15.126  | 192.168.15.127          |
| Subred 9 | 192.168.15.128  | 192.168.15.129

  | 192.168.15.142  | 192.168.15.143          |
| Subred 10| 192.168.15.144  | 192.168.15.145  | 192.168.15.158  | 192.168.15.159          |
| Subred 11| 192.168.15.160  | 192.168.15.161  | 192.168.15.174  | 192.168.15.175          |
| Subred 12| 192.168.15.176  | 192.168.15.177  | 192.168.15.190  | 192.168.15.191          |
| Subred 13| 192.168.15.192  | 192.168.15.193  | 192.168.15.206  | 192.168.15.207          |
| Subred 14| 192.168.15.208  | 192.168.15.209  | 192.168.15.222  | 192.168.15.223          |
| Subred 15| 192.168.15.224  | 192.168.15.225  | 192.168.15.238  | 192.168.15.239          |
| Subred 16| 192.168.15.240  | 192.168.15.241  | 192.168.15.254  | 192.168.15.255          |
