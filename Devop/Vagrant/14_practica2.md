### **Práctica 1: Crear una box base desde VirtualBox**

El objetivo de esta práctica es crear una máquina virtual de Ubuntu Server configurada con OpenSSH Server desde la instalación y empaquetarla como una box de Vagrant para reutilizarla.

#### **Paso 1: Descargar Ubuntu Server**
1. Ve al sitio oficial de [Ubuntu Server](https://ubuntu.com/download/server) y descarga la imagen `.iso` de la versión deseada (por ejemplo, 22.04 LTS).
2. Guarda el archivo `.iso` en una carpeta accesible.


#### **Paso 2: Crear la máquina virtual en VirtualBox**
1. Abre VirtualBox y haz clic en **"Nueva"**.
2. Configura los parámetros básicos:
   - **Nombre:** `base_ubuntu_server_24_20`. Podéis poner el nombre que queráis, luego en un paso más abajo se usa.
   - **Tipo:** Linux.
   - **Versión:** Ubuntu (64-bit).
3. **RAM y disco duro:**
   - RAM: 1024 MB (se puede cambiar luego con el `Vagrantfile`).
   - Disco duro: Dinámico, con un tamaño inicial de 10 GB.
4. **Adjunta la ISO:**
   - En la sección de almacenamiento, selecciona "Vacío".
   - Haz clic en el icono del disco y selecciona la imagen `.iso` descargada.


#### **Paso 3: Instalar Ubuntu Server**
1. Inicia la máquina y sigue el asistente de instalación:
   - **Idioma:** Selecciona tu idioma preferido.
   - **Usuario:**
     - Nombre de usuario: `vagrant`.
     - Contraseña: `vagrant`.
   - **Red:** Configura la red con DHCP (esto es suficiente para la instalación base).
   - **Paquetes:** Marca la opción para instalar **OpenSSH Server**.
2. Completa la instalación y reinicia la máquina.



#### **Paso 4: Empaquetar la máquina como una box**
1. **Apaga la máquina virtual:**
   Una vez instalada, apaga la máquina desde dentro:
   ```bash
   sudo shutdown now
   ```
2. **Empaqueta la máquina con Vagrant:**
   Desde tu máquina anfitriona, utiliza el comando:
   ```bash
   vagrant package --base Ubuntu_Base_VM --output ubuntu-server.box
   ```
   Esto crea un archivo `ubuntu-server.box` listo para usar.

