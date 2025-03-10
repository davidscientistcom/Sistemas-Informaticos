## 1. ¿Qué es una partición?

Una **partición** es una división lógica dentro de un dispositivo de almacenamiento (como un disco duro o SSD) que actúa como si fuera un disco independiente. Dicho de otro modo, el disco se “trocea” virtualmente, de manera que cada partición puede gestionar su propio sistema de ficheros y, en muchos casos, incluso su propio sistema operativo.

### 1.1 Concepto básico

- **Disco sin particionar**: Sería un disco “en bruto” que, a ojos del sistema operativo, no tiene una estructura definida para albergar datos.  
- **Disco con particiones**: El disco se divide en secciones (particiones), cada una lista para albergar un sistema de ficheros.

En la práctica, no se suele trabajar con discos sin particionar porque:
1. Los sistemas operativos necesitan al menos una partición de arranque (boot) donde instalarse.  
2. Dividir el disco en varias particiones aporta ventajas como aislar datos, permitir multiboot, facilitar el mantenimiento, etc.



## 2. ¿Para qué sirve particionar un disco?

Existen varias razones para realizar particiones en un disco:

1. **Instalar múltiples sistemas operativos**: Por ejemplo, Windows en una partición con NTFS y Linux en otra con ext4 o Btrfs.  
2. **Organizar la información**: Separar el sistema operativo de los datos de usuario (por ejemplo, particiones `/`, `/home`, `/var` en Linux).  
3. **Facilitar la seguridad o la recuperación**: Si una partición se corrompe, las demás pueden mantenerse intactas y, en algunos casos, simplifica la recuperación o reinstalación del sistema.  
4. **Limitar el crecimiento de archivos**: Cada partición tiene un tamaño máximo asignado; si se llena, no afecta a otras particiones, lo que puede ayudar a controlar el consumo de espacio.  
5. **Optimizar el rendimiento**: Algunas configuraciones específicas en servidores o discos SSD se benefician al separar tipos de datos en particiones distintas.



## 3. Tipos de tablas de particiones (MBR y GPT)

La información sobre **cómo** se dividen las particiones en un disco se almacena en algo llamado “tabla de particiones”. Existen dos esquemas principales:

### 3.1 MBR (Master Boot Record)

- **Antiguo estándar**: Fue el más utilizado en PCs durante décadas.  
- **Límite de tamaño**: Permite discos de hasta 2 TB (aprox.) y solo hasta 4 particiones primarias.  
- **Particiones primarias y extendidas**: MBR distingue entre:
  - **Particiones primarias** (máximo 4).  
  - **Partición extendida**, dentro de la cual pueden existir múltiples **particiones lógicas**.  

### 3.2 GPT (GUID Partition Table)

- **Estándar más reciente**: Reemplaza a MBR en equipos modernos (sobre todo con firmware UEFI).  
- **Soporta grandes capacidades**: Maneja discos de varios terabytes (incluso petabytes), sin el límite de 2 TB de MBR.  
- **Número amplio de particiones**: GPT permite crear muchas particiones (normalmente hasta 128 en la práctica, aunque el límite teórico es mayor).  
- **Mejor redundancia**: GPT almacena múltiples copias de la tabla de particiones en distintas ubicaciones del disco para proteger la información ante fallos.



## 4. Relación entre la partición y el sistema de ficheros

Para que un dispositivo de almacenamiento sea utilizable por el sistema operativo, a cada partición se le debe asignar un **sistema de ficheros**. La secuencia habitual es:

1. **Particionar el disco**: Crear (por ejemplo) dos particiones en un mismo disco.  
2. **Formatear** cada partición con un sistema de ficheros:  
   - Partición 1: NTFS (Windows)  
   - Partición 2: ext4 (Linux)  
3. **Montar y asignar una letra o punto de montaje**:  
   - En Windows, la Partición 1 se monta como `C:` y la Partición 2 podría no ser reconocida nativamente (necesita drivers ext4 o herramientas de terceros).  
   - En Linux, la Partición 2 podría ser montada en `/home`, `/media`, etc., mientras que la Partición 1 podría ser montada con drivers NTFS (p. ej. `ntfs-3g`).

En cada partición, el sistema de ficheros define cómo se almacenan y organizan los archivos y directorios. El motivo de que haya **incompatibilidad** entre sistemas de ficheros (NTFS vs. ext4, APFS vs. FAT32, etc.) es que cada uno usa estructuras de datos y métodos de gestión diferentes. Por tanto, el sistema operativo necesita un **controlador específico** para interpretar y trabajar con cada sistema de ficheros.



## 5. Tipos de particiones en MBR (detalles)

En caso de usar la tabla de particiones MBR, se distinguen:

1. **Partición primaria**: Son las particiones “directas” reconocidas por el firmware o el BIOS, donde se suele ubicar el sistema operativo. Se pueden tener hasta 4 particiones primarias por disco (en MBR).  
2. **Partición extendida**: Para sortear la limitación de 4, se puede marcar una de esas particiones primarias como extendida, que actúa como contenedor para **particiones lógicas**.  
3. **Partición lógica**: Existen dentro de la partición extendida. A efectos prácticos, se comportan como particiones adicionales, pero solo se pueden crear si hay una partición extendida.  

De este modo, una típica configuración MBR podría ser:
- Partición primaria 1: Windows (NTFS).  
- Partición primaria 2: Linux `/boot` (ext4).  
- Partición extendida (que incluye varias particiones lógicas):  
  - Lógica 1: Linux `/` (ext4)  
  - Lógica 2: Linux swap



## 6. Ejemplo práctico de particionado y sistemas de ficheros

Imaginemos un disco duro de 1 TB en el que queremos:

1. **Windows 10** instalado con NTFS.  
2. **Linux** (por ejemplo, Ubuntu) con ext4.  
3. Una partición de **intercambio** de datos accesible desde ambos sistemas, formateada en **exFAT** (ya que exFAT es ampliamente soportado por Windows y Linux, con las herramientas adecuadas).

La configuración posible en modo GPT podría ser:

- **Partición EFI** (pequeña, de 100-300 MB): Contiene los archivos de arranque (boot) en sistemas UEFI.  
- **Partición de Windows** (NTFS): Tamaño, por ejemplo, de 400 GB.  
- **Partición de Linux** (ext4): Otros 400 GB.  
- **Partición de datos compartidos** (exFAT): 200 GB.

Cada partición tendría su propio sistema de ficheros y su propia ruta/punto de montaje.



## 7. Herramientas para gestionar particiones

Dependiendo del sistema operativo y el tipo de tabla de particiones (MBR o GPT), se utilizan distintas **herramientas** para gestionar (crear, redimensionar o eliminar) las particiones:

- **Windows**: Utiliza “Administración de discos” (Disk Management) o la herramienta en consola `diskpart`.  
- **Linux**: Hay utilidades en consola como `fdisk` (para MBR principalmente), `gdisk` (para GPT) o `parted` (más versátil). También existen interfaces gráficas como **GParted**.  
- **macOS**: Emplea la “Utilidad de Discos” para gestionar particiones (APFS, HFS+, etc.) y volúmenes.



## 8. Novedades o tendencias recientes en el particionado

Aunque el concepto de particionar un disco es bastante antiguo, existen algunas novedades o tendencias en la forma de organizar particiones y volúmenes:

1. **GPT como estándar principal**:  
   Con la llegada de UEFI, cada vez se relega más a MBR al pasado, sobre todo por sus limitaciones de tamaño y cantidad de particiones.

2. **Uso de LVM (Logical Volume Manager) en Linux**:  
   En lugar de usar particiones fijas, LVM permite crear “volúmenes lógicos” más flexibles. Se pueden redimensionar en caliente (sin reiniciar), agregar discos adicionales al grupo de volúmenes, etc.

3. **Sistemas de ficheros avanzados con gestión interna de volúmenes**:  
   Por ejemplo, ZFS o Btrfs pueden manejar varios discos y “subvolúmenes” de modo que la partición deja de ser la unidad de organización principal. Aun así, en un entorno tradicional, se sigue necesitando al menos una partición para instalar el sistema de ficheros.

4. **Arranque seguro (Secure Boot) en UEFI**:  
   En algunos casos, la partición EFI (FAT32) contiene archivos criptográficamente firmados para garantizar la integridad del proceso de arranque.

