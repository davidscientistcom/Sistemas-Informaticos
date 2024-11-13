### Ejercicios de Identificación de IPs y Máscaras (Clase A, B y C)

1. **IP:** 10.0.0.5 | **Máscara:** 255.0.0.0  
   **Clase:** A

2. **IP:** 172.16.45.12 | **Máscara:** 255.255.0.0  
   **Clase:** B

3. **IP:** 192.168.1.25 | **Máscara:** 255.255.255.0  
   **Clase:** C

4. **IP:** 8.8.8.8 | **Máscara:** 255.0.0.0  
   **Clase:** A

5. **IP:** 172.30.250.3 | **Máscara:** 255.255.0.0  
   **Clase:** B

6. **IP:** 192.0.2.1 | **Máscara:** 255.255.255.0  
   **Clase:** C

7. **IP:** 10.10.10.10 | **Máscara:** 255.0.0.0  
   **Clase:** A

8. **IP:** 172.20.100.50 | **Máscara:** 255.255.0.0  
   **Clase:** B

9. **IP:** 192.168.15.15 | **Máscara:** 255.255.255.0  
   **Clase:** C

10. **IP:** 11.1.1.1 | **Máscara:** 255.0.0.0  
    **Clase:** A

### Ejemplo Paso a Paso

**Datos del ejercicio:**
- **IP:** 192.168.1.130
- **Máscara:** 255.255.255.0

**Objetivo:** Calcular la dirección de red correspondiente.

#### Paso 1: Convertir la IP y la máscara a formato binario

Primero, desglosamos la IP y la máscara en sus octetos y luego convertimos cada octeto a binario.

1. **IP 192.168.1.130** en binario:
   - `192` en binario es `11000000`
   - `168` en binario es `10101000`
   - `1` en binario es `00000001`
   - `130` en binario es `10000010`

   Así, la IP `192.168.1.130` en binario es:
   ```
   11000000.10101000.00000001.10000010
   ```

2. **Máscara 255.255.255.0** en binario:
   - `255` en binario es `11111111`
   - `255` en binario es `11111111`
   - `255` en binario es `11111111`
   - `0` en binario es `00000000`

   Así, la máscara `255.255.255.0` en binario es:
   ```
   11111111.11111111.11111111.00000000
   ```

#### Paso 2: Realizar una operación AND bit a bit entre la IP y la máscara

La operación AND toma cada bit de la IP y de la máscara y devuelve `1` solo si ambos bits son `1`. De lo contrario, devuelve `0`.

Realizamos la operación AND bit a bit entre la IP y la máscara en binario:

| IP (binario)              | 11000000 | 10101000 | 00000001 | 10000010 |
|---------------------------|----------|----------|----------|----------|
| Máscara (binario)         | 11111111 | 11111111 | 11111111 | 00000000 |
| **Resultado (Dirección de Red)** | **11000000** | **10101000** | **00000001** | **00000000** |

#### Paso 3: Convertir el resultado de la operación AND a decimal

Ahora, convertimos el resultado de la operación AND de cada octeto a su representación decimal para obtener la dirección de red en formato decimal.

1. `11000000` en decimal es `192`
2. `10101000` en decimal es `168`
3. `00000001` en decimal es `1`
4. `00000000` en decimal es `0`

#### Paso 4: Escribir la dirección de red

La dirección de red en formato decimal es:
```
192.168.1.0
```

#### Resumen del ejercicio

Para la IP `192.168.1.130` con la máscara `255.255.255.0`, la dirección de red es **192.168.1.0**.

Este enfoque paso a paso ayuda a entender cómo la máscara de subred se utiliza para "enmascarar" los bits de la IP, dejando solo los bits de la red. Es ideal para que los estudiantes practiquen con varias IPs y máscaras, viendo cómo la estructura binaria revela la red a la que pertenece cada IP.

### Ejercicios para Calcular la Dirección de Red

1. **IP:** 192.168.1.100 | **Máscara:** 255.255.255.0  
   **Dirección de Red:** 192.168.1.0

2. **IP:** 172.16.50.5 | **Máscara:** 255.255.0.0  
   **Dirección de Red:** 172.16.0.0

3. **IP:** 10.0.5.30 | **Máscara:** 255.0.0.0  
   **Dirección de Red:** 10.0.0.0

4. **IP:** 192.0.2.15 | **Máscara:** 255.255.255.0  
   **Dirección de Red:** 192.0.2.0

5. **IP:** 172.20.10.100 | **Máscara:** 255.255.0.0  
   **Dirección de Red:** 172.20.0.0

6. **IP:** 10.10.10.10 | **Máscara:** 255.0.0.0  
   **Dirección de Red:** 10.0.0.0

7. **IP:** 192.168.100.200 | **Máscara:** 255.255.255.0  
   **Dirección de Red:** 192.168.100.0

8. **IP:** 172.31.99.254 | **Máscara:** 255.255.0.0  
   **Dirección de Red:** 172.31.0.0

9. **IP:** 8.8.8.8 | **Máscara:** 255.0.0.0  
   **Dirección de Red:** 8.0.0.0

10. **IP:** 192.168.2.130 | **Máscara:** 255.255.255.0  
    **Dirección de Red:** 192.168.2.0


### Ejercicios de Subnetting Rápidos

1. **IP:** 192.168.1.10/24  
   **Dirección de Red:** 192.168.1.0  
   **Primer Host:** 192.168.1.1  
   **Último Host:** 192.168.1.254  
   **Broadcast:** 192.168.1.255

2. **IP:** 10.0.0.50/8  
   **Dirección de Red:** 10.0.0.0  
   **Primer Host:** 10.0.0.1  
   **Último Host:** 10.255.255.254  
   **Broadcast:** 10.255.255.255

3. **IP:** 172.16.50.1/16  
   **Dirección de Red:** 172.16.0.0  
   **Primer Host:** 172.16.0.1  
   **Último Host:** 172.16.255.254  
   **Broadcast:** 172.16.255.255

4. **IP:** 192.168.10.25/28  
   **Dirección de Red:** 192.168.10.16  
   **Primer Host:** 192.168.10.17  
   **Último Host:** 192.168.10.30  
   **Broadcast:** 192.168.10.31

5. **IP:** 10.10.10.5/20  
   **Dirección de Red:** 10.10.0.0  
   **Primer Host:** 10.10.0.1  
   **Último Host:** 10.10.15.254  
   **Broadcast:** 10.10.15.255

6. **IP:** 192.168.5.100/25  
   **Dirección de Red:** 192.168.5.0  
   **Primer Host:** 192.168.5.1  
   **Último Host:** 192.168.5.126  
   **Broadcast:** 192.168.5.127

7. **IP:** 172.18.12.45/23  
   **Dirección de Red:** 172.18.12.0  
   **Primer Host:** 172.18.12.1  
   **Último Host:** 172.18.13.254  
   **Broadcast:** 172.18.13.255

8. **IP:** 192.168.1.75/26  
   **Dirección de Red:** 192.168.1.64  
   **Primer Host:** 192.168.1.65  
   **Último Host:** 192.168.1.126  
   **Broadcast:** 192.168.1.127

9. **IP:** 10.20.30.40/18  
   **Dirección de Red:** 10.20.0.0  
   **Primer Host:** 10.20.0.1  
   **Último Host:** 10.20.63.254  
   **Broadcast:** 10.20.63.255

10. **IP:** 192.168.2.200/29  
    **Dirección de Red:** 192.168.2.200  
    **Primer Host:** 192.168.2.201  
    **Último Host:** 192.168.2.206  
    **Broadcast:** 192.168.2.207
