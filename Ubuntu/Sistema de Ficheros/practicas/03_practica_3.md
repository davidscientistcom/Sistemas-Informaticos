
## PRÁCTICA 3: Gestión de volúmenes lógicos y snapshots (LVM en Linux)

### Objetivo
Aprender a crear y gestionar volúmenes lógicos (LVM), y realizar snapshots para copias puntuales de seguridad o pruebas.

### Requisitos
- Un disco o particiones sin uso para crear Physical Volumes.  
- Paquete `lvm2` instalado (normalmente disponible en la mayoría de distribuciones).

### Pasos

1. **Crear “Physical Volumes” (PV)**  
   - Identificar particiones a usar para LVM (por ejemplo `/dev/sdb1` y `/dev/sdc1`).  
   - Convertirlas en PV:
     ```bash
     sudo pvcreate /dev/sdb1 /dev/sdc1
     ```
   - Verificar con `pvs`.

2. **Crear un “Volume Group” (VG)**  
   - Agrupar esos PV en un grupo de volúmenes, por ejemplo `vg_datos`:
     ```bash
     sudo vgcreate vg_datos /dev/sdb1 /dev/sdc1
     ```
   - Verificar con `vgs`.

3. **Crear un “Logical Volume” (LV)**  
   - Definir el tamaño (por ejemplo 20 GB) para un LV llamado `lv_almacen`:
     ```bash
     sudo lvcreate -n lv_almacen -L 20G vg_datos
     ```
   - Verificar con `lvs`.

4. **Formatear y montar el LV**  
   - Formatear en ext4:
     ```bash
     sudo mkfs.ext4 /dev/vg_datos/lv_almacen
     ```
   - Crear carpeta de montaje, p. ej. `/mnt/almacen`:
     ```bash
     sudo mkdir /mnt/almacen
     sudo mount /dev/vg_datos/lv_almacen /mnt/almacen
     ```
   - Añadir a `/etc/fstab` si se desea montaje automático:
     ```
     /dev/vg_datos/lv_almacen   /mnt/almacen   ext4   defaults   0 2
     ```

5. **Ampliar el LV**  
   - Supongamos que necesitamos 10 GB más. Ejecutar:
     ```bash
     sudo lvextend -L +10G /dev/vg_datos/lv_almacen
     sudo resize2fs /dev/vg_datos/lv_almacen
     ```
   - Comprobar con `df -h` que el tamaño ha aumentado.

6. **Crear un snapshot**  
   - Para congelar el estado de un LV en un momento dado, por ejemplo:
     ```bash
     sudo lvcreate -s -n snap_almacen -L 1G /dev/vg_datos/lv_almacen
     ```
   - Esto crea un snapshot llamado `snap_almacen`. Se puede montar en `/mnt/almacen_snap` para ver el contenido tal como estaba al momento del snapshot:
     ```bash
     sudo mkdir /mnt/almacen_snap
     sudo mount /dev/vg_datos/snap_almacen /mnt/almacen_snap
     ```
   - Al borrar o hacer cambios en `lv_almacen`, el snapshot conserva los datos originales hasta que se elimina.

### Resultado
Habrás creado un entorno LVM con al menos un volumen lógico formateado y un snapshot. Este proceso muestra la potencia de LVM para escalar el almacenamiento y realizar copias puntuales.

