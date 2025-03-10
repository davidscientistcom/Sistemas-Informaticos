## PRÁCTICA 1: Gestión de particiones en Linux

### Objetivo
Aprender a crear, eliminar, redimensionar y formatear particiones en un disco duro en Linux usando herramientas como `fdisk`, `parted` o “GParted” (si preferimos entorno gráfico).

### Requisitos previos
- Un disco secundario o un espacio disponible en disco (por ejemplo, una máquina virtual) para no afectar la instalación principal.
- Herramientas de particionado instaladas (p. ej., `fdisk`, `gdisk`, `parted` o GParted).

### Pasos

1. **Identificar el disco que se va a particionar**  
   - Desde consola, usar `lsblk`, `fdisk -l` o `blkid` para localizar el disco deseado (p. ej., `/dev/sdb`).

2. **Iniciar la herramienta de particionado**  
   - Por ejemplo, con `fdisk /dev/sdb` (o `sudo fdisk /dev/sdb`).

3. **Crear tabla de particiones (MBR o GPT)**  
   - En `fdisk`, dentro de la consola interactiva, se pueden usar comandos como `g` para crear una tabla de particiones GPT (o `o` para MBR).  
   - Si se prefiere GPT, también se puede usar `gdisk` o `parted`.

4. **Crear una partición primaria**  
   - Ejemplo en `fdisk`:
     - Comando `n` (new partition).  
     - Elegir el tipo (primary: `p`).  
     - Seleccionar el número de partición (p.ej. `1`).  
     - Indicar el tamaño (p. ej., `+10G` para 10 GB).  
   - Repetir para crear más particiones (hasta 4 en MBR como primarias, o las que se deseen en GPT).

5. **Guardar cambios**  
   - En `fdisk`, se hace con `w` (write) para escribir la tabla de particiones y salir.

6. **Formatear la partición**  
   - Supongamos que la nueva partición es `/dev/sdb1`. Elegimos un sistema de ficheros, por ejemplo `ext4`:  
     ```bash
     sudo mkfs.ext4 /dev/sdb1
     ```
   - Para NTFS (con herramientas de Linux), podríamos hacer `sudo mkfs.ntfs /dev/sdb1` (necesitando `ntfs-3g` instalado, si no viene por defecto).

7. **Montar la partición**  
   - Crear un punto de montaje, por ejemplo `/mnt/practica1`:  
     ```bash
     sudo mkdir -p /mnt/practica1
     sudo mount /dev/sdb1 /mnt/practica1
     ```
   - Verificar con `df -h` que `/dev/sdb1` está montado.

8. **Opcional: Añadir al fichero `/etc/fstab`**  
   - Para que se monte automáticamente al iniciar el sistema, editar `/etc/fstab` y añadir una línea similar a:  
     ```
     /dev/sdb1    /mnt/practica1    ext4    defaults    0 2
     ```
     (Ajustar según partición, punto de montaje y sistema de ficheros).

### Resultado
Tendremos un disco con una o varias particiones nuevas, formateadas con el sistema de ficheros escogido, y montadas correctamente en Linux.

