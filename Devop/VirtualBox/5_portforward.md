Para realizar un **port forwarding** (redireccionamiento de puertos) en VirtualBox y acceder a un servicio de la máquina virtual configurada con adaptador NAT y Host Only, puedes seguir estos pasos. La redirección de puertos permite que accedas a los servicios en la máquina virtual desde tu host usando la IP del host en el puerto que especifiques.

### Configurar el Port Forwarding en VirtualBox

1. **Accede a la configuración de la máquina virtual:**
   - Selecciona la máquina virtual en VirtualBox.
   - Ve a **Configuración** > **Red**.

2. **Configura el adaptador NAT:**
   - Asegúrate de que uno de los adaptadores esté configurado en modo **NAT**.
   - Haz clic en **Avanzado** y selecciona **Redirección de puertos** o **Port Forwarding**.

3. **Agrega una regla de redirección de puertos:**
   - Haz clic en el botón de **Agregar regla** (+).
   - Configura los parámetros de la siguiente forma:
     - **Nombre:** Puedes dar un nombre que te ayude a identificar el servicio (por ejemplo, `SSH` o `HTTP`).
     - **Protocolo:** Selecciona `TCP` o `UDP` según el servicio (por ejemplo, TCP para SSH o HTTP).
     - **Puerto del host:** Indica el puerto de tu host que deseas utilizar (por ejemplo, `2222` para SSH o `8080` para HTTP).
     - **IP de la máquina:** Déjalo en blanco o en `127.0.0.1` para que se aplique a todas las interfaces.
     - **Puerto invitado:** Especifica el puerto donde se encuentra el servicio en la máquina virtual (por ejemplo, `22` para SSH o `80` para HTTP).
   
   Ejemplo para un servicio SSH:
   - **Nombre:** SSH
   - **Protocolo:** TCP
   - **Puerto del host:** 2222
   - **IP del anfitrión:** Deja este campo en blanco
   - **Puerto invitado:** 22
   - **IP del invitado:** También puedes dejarlo en blanco o poner la IP estática configurada (por ejemplo, `192.168.56.10`).

4. **Guarda la configuración** y cierra la ventana.

### Acceder al Servicio desde el Host

Ahora puedes acceder al servicio desde el host usando `localhost` y el **puerto del host** que especificaste en la configuración:

- **SSH:**  
  ```
  ssh usuario@localhost -p 2222
  ```
- **HTTP:**  
  Abre un navegador y dirígete a `http://localhost:8080` (si configuraste la redirección para HTTP en el puerto 8080).

### Notas importantes

- **Limitaciones del NAT**: La configuración NAT permite que solo el host acceda al servicio redirigido desde la máquina virtual, lo cual es ideal para pruebas locales.
- **Compatibilidad de puertos**: Verifica que el puerto configurado en el host (como `2222` o `8080`) no esté en uso por otros servicios, ya que esto podría causar conflictos.

Esta configuración permite combinar el uso de una IP estática en Host Only para comunicación directa entre la máquina virtual y el host, mientras que NAT con redirección de puertos te facilita acceder a servicios específicos desde el host sin comprometer la seguridad de la red principal.