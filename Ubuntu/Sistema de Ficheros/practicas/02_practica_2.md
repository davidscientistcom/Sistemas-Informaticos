## PRÁCTICA 2: Acceso cruzado entre particiones (Windows <-> Linux)

### Objetivo
Practicar cómo acceder a particiones con distintos sistemas de ficheros desde Windows a ext4 y desde Linux a NTFS, cuando coexisten en un mismo equipo o disco.

### Escenario típico
Un equipo con doble arranque (dual boot) con **Windows** y **Linux**, o un disco USB externo con varios sistemas de ficheros.

#### 2.1 Acceder a una partición ext4 desde Windows

1. **Herramientas necesarias**  
   - Windows no soporta ext4 de forma nativa. Por tanto, se requiere software de terceros:
     - Ejemplo: [Ext2Fsd](http://www.ext2fsd.com/) (soporta ext2, ext3 y ext4 en lectura/escritura limitada).  
     - Otras alternativas similares (Paragon extFS, Linux File Systems for Windows, etc.).

2. **Instalación y montaje**  
   - Instalar la herramienta en Windows.  
   - Conectado el disco (o la partición) ext4, abrir el programa y montar la partición.  
   - Asignar una letra de unidad (p. ej. `E:`).  
   - Ya se puede acceder a los archivos desde el Explorador de Windows (aunque con limitaciones en permisos y caracteres especiales en nombres de archivo).

#### 2.2 Acceder a una partición NTFS desde Linux

1. **Herramientas necesarias**  
   - Muchas distros de Linux traen preinstalado **ntfs-3g** (controlador FUSE para NTFS).  
   - Si no, instalarlo con `sudo apt-get install ntfs-3g` (Debian/Ubuntu) o similar en otras distros.

2. **Montar la partición NTFS**  
   - Identificar el dispositivo (por ejemplo `/dev/sda2`).  
   - Crear punto de montaje: `sudo mkdir /mnt/windows`.  
   - Montar:  
     ```bash
     sudo mount -t ntfs-3g /dev/sda2 /mnt/windows
     ```
   - Verificar con `df -h`.  
   - Para montaje automático, editar `/etc/fstab` añadiendo la línea:  
     ```
     /dev/sda2   /mnt/windows   ntfs-3g   defaults,uid=1000,gid=1000,umask=0022   0 0
     ```
     (Ajustar permisos, usuario y grupo según sea necesario).

### Resultado
Se habrá conseguido que ambos sistemas (Windows y Linux) accedan a las particiones mutuas (ext4 y NTFS), aunque Windows necesite software adicional para ext4. Esto permite compartir datos a través de un disco que contiene distintos sistemas de ficheros.


