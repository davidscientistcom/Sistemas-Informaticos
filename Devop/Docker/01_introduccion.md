# 1. ¿Qué es Docker?

## 1.1. Definición y Concepto Básico

**Docker** es una plataforma de código abierto que permite desarrollar, distribuir y ejecutar aplicaciones en contenedores. Los *contenedores* son entornos aislados que agrupan la aplicación junto con todas sus dependencias (bibliotecas, configuraciones, etc.), lo que garantiza que la aplicación se ejecutará de la misma forma independientemente del entorno en el que se despliegue.

### Características Clave:
- **Ligereza:** Los contenedores comparten el núcleo del sistema operativo del host, lo que reduce el uso de recursos en comparación con las máquinas virtuales.
- **Portabilidad:** Un contenedor se puede ejecutar en cualquier sistema que tenga instalado Docker, lo que facilita la migración entre entornos de desarrollo, prueba y producción.
- **Rapidez:** La creación y el inicio de un contenedor son procesos casi instantáneos, en contraste con el arranque de una máquina virtual.
- **Aislamiento:** Cada contenedor se ejecuta de forma aislada, evitando conflictos entre aplicaciones y facilitando la administración de dependencias.

## 1.2. Componentes Principales de Docker

- **Docker Engine:** Es el motor de contenedores que se encarga de crear y gestionar los contenedores.
- **Imágenes (Images):** Son plantillas inmutables que contienen todo lo necesario para ejecutar una aplicación (sistema operativo base, bibliotecas, código de la aplicación, etc.). Las imágenes se pueden compartir a través de repositorios como [Docker Hub](https://hub.docker.com/).
- **Contenedores (Containers):** Son instancias en ejecución de las imágenes. Se crean, inician, detienen y eliminan según las necesidades del usuario.
- **Dockerfile:** Es un archivo de texto que contiene instrucciones para construir una imagen de Docker. Permite automatizar la creación de imágenes definiendo, paso a paso, cómo se debe configurar el entorno.

### Ejemplo Práctico: Un Dockerfile Simple

```dockerfile
# Usamos una imagen base de Ubuntu
FROM ubuntu:20.04

# Actualizamos la lista de paquetes e instalamos Python
RUN apt-get update && apt-get install -y python3

# Copiamos el código de la aplicación al contenedor
COPY app.py /usr/src/app/app.py

# Establecemos el directorio de trabajo
WORKDIR /usr/src/app

# Indicamos el comando que se ejecutará al iniciar el contenedor
CMD ["python3", "app.py"]
```

En este ejemplo, se parte de una imagen base de Ubuntu, se instalan dependencias y se copia el código de la aplicación. Finalmente, se define el comando que se ejecutará cuando se inicie el contenedor.



# 2. Diferencias Entre Docker y la Virtualización con Máquinas Virtuales

La virtualización tradicional, a través de **máquinas virtuales (VMs)**, y la **containerización** con Docker son dos enfoques para aislar y ejecutar aplicaciones, pero presentan diferencias fundamentales.

## 2.1. Arquitectura y Funcionamiento

- **Máquinas Virtuales:**
  - **Virtualización Completa:** Cada VM incluye no solo la aplicación y sus dependencias, sino también un sistema operativo completo. Esto se logra mediante un hipervisor (como VMware, VirtualBox o Hyper-V) que abstrae el hardware físico.
  - **Recursos:** Debido a que cada VM contiene su propio sistema operativo, el consumo de recursos (CPU, memoria y almacenamiento) es mayor.
  - **Arranque:** El proceso de inicio de una VM suele ser más lento debido a la carga completa del sistema operativo.

- **Contenedores con Docker:**
  - **Virtualización a Nivel de Sistema Operativo:** Los contenedores comparten el núcleo del sistema operativo del host. Esto elimina la necesidad de replicar un sistema operativo completo en cada contenedor.
  - **Ligereza:** Al no requerir un sistema operativo completo, los contenedores son mucho más ligeros y consumen menos recursos.
  - **Arranque Rápido:** La inicialización de un contenedor es casi instantánea, lo que facilita la escalabilidad y la gestión de aplicaciones en entornos dinámicos.

## 2.2. Comparación en Forma de Tabla

| Característica          | Máquinas Virtuales                      | Contenedores (Docker)                    |
|-------------------------|-----------------------------------------|------------------------------------------|
| **Aislamiento**         | Completo, incluye el sistema operativo  | Aislamiento a nivel de proceso y sistema |
| **Uso de Recursos**     | Alto consumo (OS completo)              | Ligero (comparten el núcleo)             |
| **Tiempo de Arranque**  | Lento (carga del SO completo)           | Muy rápido                               |
| **Portabilidad**        | Menos portable entre diferentes hipervisores | Alta portabilidad en cualquier host con Docker |
| **Mantenimiento**       | Mayor complejidad en actualizaciones del SO | Más sencillo, enfocado en la aplicación    |

### Ejemplo Comparativo:

- **Caso de Uso con Máquinas Virtuales:** Una empresa que requiere aislar completamente sistemas operativos por razones de seguridad y compatibilidad puede optar por VMs, aunque esto implique mayor consumo de recursos.
- **Caso de Uso con Docker:** Una startup que necesita desplegar rápidamente microservicios y escalar de forma dinámica puede beneficiarse enormemente de la rapidez y eficiencia de los contenedores.


# 3. Instalación de Docker

La instalación de Docker varía según el sistema operativo. A continuación, se describen los pasos básicos para instalar Docker en **Windows** y **Ubuntu**.

## 3.1. Instalación en Windows

### Requisitos Previos:
- **Sistema Operativo:** Windows 10 64-bit o Windows 11.
- **Virtualización:** Es necesario contar con soporte para virtualización (Hyper-V o WSL2).
- **WSL2:** Se recomienda usar el Subsistema de Windows para Linux (WSL2) para un mejor rendimiento.

### Pasos de Instalación:

1. **Descargar Docker Desktop:**
   - Accede a la [página oficial de Docker Desktop](https://www.docker.com/products/docker-desktop) y descarga el instalador para Windows.

2. **Instalación:**
   - Ejecuta el instalador descargado y sigue las instrucciones del asistente.
   - Durante la instalación, asegúrate de habilitar la opción para usar **WSL2** si se te solicita. Esto puede implicar la instalación de una distribución de Linux (por ejemplo, Ubuntu) desde Microsoft Store.

3. **Configuración Inicial:**
   - Una vez instalada la aplicación, inicia Docker Desktop.
   - Verifica que Docker se esté ejecutando correctamente y, desde una ventana de PowerShell o CMD, ejecuta:
     ```powershell
     docker --version
     ```
   - Deberías ver la versión de Docker instalada, lo que indica que la instalación ha sido exitosa.

4. **Ejemplo Práctico:**
   - Ejecuta un contenedor de prueba:
     ```powershell
     docker run hello-world
     ```
   - Este comando descarga una imagen de prueba y la ejecuta, mostrando un mensaje de confirmación.

## 3.2. Instalación en Ubuntu

### Requisitos Previos:
- **Sistema Operativo:** Ubuntu (preferiblemente versiones LTS, por ejemplo, 20.04 o 22.04).
- **Permisos de Superusuario:** Se requiere acceso a una cuenta con privilegios de `sudo`.

### Pasos de Instalación:

1. **Actualizar el Índice de Paquetes:**
   ```bash
   sudo apt-get update
   ```

2. **Instalar Dependencias Requeridas:**
   ```bash
   sudo apt-get install apt-transport-https ca-certificates curl software-properties-common
   ```

3. **Añadir la Clave GPG Oficial de Docker:**
   ```bash
   curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /usr/share/keyrings/docker-archive-keyring.gpg
   ```

4. **Configurar el Repositorio de Docker:**
   ```bash
   echo \
     "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/docker-archive-keyring.gpg] https://download.docker.com/linux/ubuntu \
     $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
   ```

5. **Actualizar el Índice de Paquetes Nuevamente:**
   ```bash
   sudo apt-get update
   ```

6. **Instalar Docker Engine:**
   ```bash
   sudo apt-get install docker-ce docker-ce-cli containerd.io
   ```

7. **Verificar la Instalación:**
   - Ejecuta:
     ```bash
     docker --version
     ```
   - Para probar la instalación, ejecuta el contenedor de ejemplo:
     ```bash
     sudo docker run hello-world
     ```
   - Deberías ver un mensaje que confirma que Docker se instaló y funciona correctamente.

8. **(Opcional) Configurar Permisos para Ejecutar Docker sin `sudo`:**
   - Añade tu usuario al grupo `docker`:
     ```bash
     sudo usermod -aG docker $USER
     ```
   - Cierra la sesión y vuelve a iniciarla para que los cambios surtan efecto. Con esto, podrás ejecutar comandos de Docker sin necesidad de anteponer `sudo`.

