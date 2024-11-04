Para gestionar los permisos en archivos y directorios en un sistema Linux, se utiliza el comando `chmod` (abreviación de *change mode*). Este comando permite especificar quién puede leer, escribir o ejecutar un archivo o directorio.

### 1. Comprensión de los Permisos en Linux

En Linux, cada archivo y directorio tiene tres tipos de permisos:

- **r (read)** - Permiso de lectura: permite ver el contenido del archivo o listar el contenido del directorio.
- **w (write)** - Permiso de escritura: permite modificar el contenido del archivo o añadir/quitar archivos en el directorio.
- **x (execute)** - Permiso de ejecución: permite ejecutar el archivo (si es un script o programa) o acceder a los contenidos del directorio.

Cada archivo o directorio también tiene tres categorías de usuarios:

1. **Usuario (owner)**: el propietario del archivo.
2. **Grupo**: el grupo al que pertenece el archivo.
3. **Otros**: cualquier usuario que no es el propietario ni pertenece al grupo.

### 2. Cambiar Permisos con `chmod`

`chmod` permite cambiar los permisos de archivos y directorios de dos maneras principales: utilizando números (notación octal) o utilizando letras (notación simbólica).

---

### 3. Método 1: `chmod` con Notación Numérica (Octal)

En la notación numérica, cada permiso se representa con un número:

- **r** (lectura) = 4
- **w** (escritura) = 2
- **x** (ejecución) = 1

Para asignar permisos, sumamos estos valores. Así, cada conjunto de permisos se expresa con un solo número:

- **0** = Sin permisos
- **1** = Ejecución (`--x`)
- **2** = Escritura (`-w-`)
- **3** = Escritura y ejecución (`-wx`)
- **4** = Lectura (`r--`)
- **5** = Lectura y ejecución (`r-x`)
- **6** = Lectura y escritura (`rw-`)
- **7** = Lectura, escritura y ejecución (`rwx`)

Cada archivo o directorio tiene tres secciones de permisos, una para cada categoría de usuario: **propietario**, **grupo** y **otros**. Los números se especifican en este orden: `chmod XYZ archivo`, donde:

- **X** es el permiso para el propietario.
- **Y** es el permiso para el grupo.
- **Z** es el permiso para otros usuarios.

#### Ejemplos de `chmod` con Notación Numérica

1. **Permiso total para todos los usuarios (777)**:

   ```bash
   chmod 777 archivo
   ```

   - **7** para el propietario (lectura, escritura y ejecución).
   - **7** para el grupo (lectura, escritura y ejecución).
   - **7** para otros (lectura, escritura y ejecución).

   Esto permite a cualquiera leer, escribir y ejecutar el archivo.

2. **Permiso de solo lectura para todos (444)**:

   ```bash
   chmod 444 archivo
   ```

   - **4** para el propietario (solo lectura).
   - **4** para el grupo (solo lectura).
   - **4** para otros (solo lectura).

   Esto da acceso de solo lectura al archivo para todos los usuarios.

3. **Lectura y escritura para el propietario, y solo lectura para el grupo y otros (644)**:

   ```bash
   chmod 644 archivo
   ```

   - **6** para el propietario (lectura y escritura).
   - **4** para el grupo (solo lectura).
   - **4** para otros (solo lectura).

   Esto es común para archivos de configuración donde solo el propietario necesita modificar el archivo.

4. **Directorio de solo lectura y ejecución para el grupo, permisos completos para el propietario (750)**:

   ```bash
   chmod 750 directorio
   ```

   - **7** para el propietario (lectura, escritura y ejecución).
   - **5** para el grupo (lectura y ejecución).
   - **0** para otros (sin permisos).

   Esto permite que solo el propietario modifique el contenido del directorio, mientras que el grupo puede acceder a su contenido.

---

### 4. Método 2: `chmod` con Notación Simbólica

En la notación simbólica, se utilizan letras para añadir o eliminar permisos:

- **u**: usuario (propietario).
- **g**: grupo.
- **o**: otros.
- **a**: todos (equivalente a `ugo`).

Los permisos se modifican usando:

- `+` para añadir un permiso.
- `-` para eliminar un permiso.
- `=` para asignar exactamente los permisos especificados.

#### Ejemplos de `chmod` con Notación Simbólica

1. **Dar permiso de ejecución al propietario**:

   ```bash
   chmod u+x archivo
   ```

   Esto añade el permiso de ejecución (`x`) solo para el propietario (`u`), manteniendo los demás permisos sin cambios.

2. **Quitar permisos de escritura para otros usuarios**:

   ```bash
   chmod o-w archivo
   ```

   Esto elimina el permiso de escritura (`w`) para otros usuarios (`o`), sin afectar los permisos del propietario y del grupo.

3. **Permitir lectura y escritura para el grupo**:

   ```bash
   chmod g+rw archivo
   ```

   Este comando añade permisos de lectura (`r`) y escritura (`w`) para el grupo (`g`), manteniendo los otros permisos sin cambios.

4. **Remover todos los permisos para otros usuarios**:

   ```bash
   chmod o= archivo
   ```

   Esto elimina todos los permisos (`=`) para otros usuarios (`o`).

5. **Asignar permisos de lectura, escritura y ejecución a todos**:

   ```bash
   chmod a+rwx archivo
   ```

   Esto otorga permisos completos (lectura, escritura y ejecución) a todos los usuarios (`a`), sobrescribiendo cualquier configuración anterior.

---

### 5. Ejemplos Adicionales para Practicar

#### Ejemplo 1: Crear un archivo y darle permisos de solo lectura para el propietario

```bash
touch archivo_privado.txt
chmod 400 archivo_privado.txt
```

Esto otorga permiso de solo lectura al propietario y niega el acceso a todos los demás.

#### Ejemplo 2: Crear un directorio con permisos de solo lectura para todos los usuarios

```bash
mkdir carpeta_publica
chmod 555 carpeta_publica
```

Todos los usuarios pueden leer el contenido de `carpeta_publica` y navegar dentro del directorio, pero no pueden modificar ni añadir contenido.

#### Ejemplo 3: Crear un archivo ejecutable y darle permisos de ejecución solo al grupo

```bash
touch script_grupo.sh
chmod g+x script_grupo.sh
```

El archivo se puede ejecutar por los usuarios que pertenecen al mismo grupo, pero el propietario y otros usuarios no tienen permisos de ejecución.

---

### Resumen Rápido de Notación Numérica y Simbólica

- **Notación Numérica**: Se usa un número de tres cifras para representar los permisos de propietario, grupo y otros (por ejemplo, `chmod 755 archivo`).
- **Notación Simbólica**: Se usa una combinación de letras y símbolos para modificar permisos sin alterar otros permisos existentes (por ejemplo, `chmod u+x archivo`).
