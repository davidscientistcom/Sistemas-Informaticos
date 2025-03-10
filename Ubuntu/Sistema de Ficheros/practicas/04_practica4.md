
## PRÁCTICA 4: Práctica usando Python para estudiar el particionado y LVM

En esta última práctica, daremos un **enfoque introductorio** para usar Python en la manipulación (o, al menos, visualización) de particiones y volúmenes lógicos. No es muy común modificar particiones directamente con Python, pero podemos usar llamadas al sistema o librerías especializadas para consultar información.

### Objetivo
- Listar discos y particiones desde Python.  
- Ejecutar comandos del sistema (por ejemplo, `lsblk`, `lvs`, `pvs`, `vgs`) y parsear su salida para mostrarlos de forma amigable.  
- (Opcionalmente, *bajo tu responsabilidad*) crear o montar un volumen mediante llamadas a `subprocess`.

### Requisitos
- Python 3 instalado.  
- Librerías estándar de Python (especialmente `subprocess` para ejecutar comandos).  
- Tener privilegios de superusuario (sudo) si se quiere manipular particiones o volúmenes.

### Pasos

1. **Crear un archivo Python**  
   - Por ejemplo, `gestor_storage.py`.

2. **Listar particiones con `lsblk`**  
   ```python
   import subprocess

   def listar_particiones():
       cmd = ["lsblk", "-o", "NAME,SIZE,FSTYPE,MOUNTPOINT"]
       result = subprocess.run(cmd, capture_output=True, text=True)
       print("Listado de particiones:\n", result.stdout)

   if __name__ == "__main__":
       listar_particiones()
   ```

   - Al ejecutar `python gestor_storage.py`, se mostrará algo parecido a:
     ```
     NAME   SIZE FSTYPE MOUNTPOINT
     sda    100G        
     ├─sda1  50G ext4   /
     └─sda2  50G ext4   /home
     ...
     ```

3. **Listar información de LVM**  
   - Podemos hacer funciones similares para `pvs`, `vgs` y `lvs`:
     ```python
     def listar_lvm_info():
         for cmd in [["pvs"], ["vgs"], ["lvs"]]:
             result = subprocess.run(cmd, capture_output=True, text=True)
             print(f"Salida de {' '.join(cmd)}:\n", result.stdout)

     if __name__ == "__main__":
         listar_particiones()
         listar_lvm_info()
   ```

4. **(Opcional) Crear un volumen lógico desde Python**  
   - Se requeriría usar `sudo lvcreate ...`. Por ejemplo:
     ```python
     def crear_lv(vg_name, lv_name, size):
         cmd = ["sudo", "lvcreate", "-n", lv_name, "-L", size, vg_name]
         subprocess.run(cmd, check=True)
         print(f"LV {lv_name} creado exitosamente en {vg_name} con tamaño {size}")
     ```
   - **Aviso**: Esto es muy delicado en un entorno real, pues cualquier error podría causar pérdidas de datos.

5. **Ejecutar y validar**  
   - Probar el script en un entorno de pruebas.  
   - Verificar que la salida coincide con la configuración real del sistema.

### Resultado
- Se obtiene un pequeño **script** en Python capaz de interactuar con el sistema para mostrar (e incluso gestionar, si así se programa) particiones y volúmenes lógicos.
- Se consolida el aprendizaje de los conceptos de particionado y LVM, integrándolo con un lenguaje de scripting que podría automatizar tareas de administración de sistemas.

