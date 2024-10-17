# 50 Ejercicios resueltos de gestión de archivos y directorios en Ubuntu por consola

**Nota:** Utiliza el comando `pwd` para saber en qué directorio te encuentras y `ls` para listar el contenido del directorio actual.

---

## Ejercicio 1: Crear un directorio simple

**Objetivo:** Crear un directorio llamado `proyecto`.

**Solución:**

```bash
$ mkdir proyecto
```

---

## Ejercicio 2: Crear múltiples directorios

**Objetivo:** Dentro de `proyecto`, crear los directorios `doc`, `src` y `bin`.

**Solución:**

```bash
$ cd proyecto
$ mkdir doc src bin
```

---

## Ejercicio 3: Crear un archivo vacío

**Objetivo:** Crear un archivo vacío llamado `README.md` dentro de `proyecto`.

**Solución:**

```bash
$ touch README.md
```

---

## Ejercicio 4: Mover un archivo a un directorio

**Objetivo:** Mover `README.md` al directorio `doc`.

**Solución:**

```bash
$ mv README.md doc/
```

---

## Ejercicio 5: Renombrar un archivo

**Objetivo:** Renombrar `README.md` a `LEEME.md` dentro de `doc`.

**Solución:**

```bash
$ mv doc/README.md doc/LEEME.md
```

---

## Ejercicio 6: Crear una estructura de directorios anidada

**Objetivo:** Dentro de `src`, crear los subdirectorios `app` y `lib`.

**Solución:**

```bash
$ mkdir -p src/app src/lib
```

---

## Ejercicio 7: Copiar un archivo

**Objetivo:** Copiar `LEEME.md` desde `doc` hasta `src/app`.

**Solución:**

```bash
$ cp doc/LEEME.md src/app/
```

---

## Ejercicio 8: Crear múltiples archivos

**Objetivo:** En `src/app`, crear los archivos `main.py`, `utils.py` y `config.py`.

**Solución:**

```bash
$ touch src/app/main.py src/app/utils.py src/app/config.py
```

---

## Ejercicio 9: Mover múltiples archivos a otro directorio

**Objetivo:** Mover todos los archivos `.py` desde `src/app` a `src/lib`.

**Solución:**

```bash
$ mv src/app/*.py src/lib/
```

---

## Ejercicio 10: Eliminar un archivo

**Objetivo:** Eliminar el archivo `config.py` de `src/lib`.

**Solución:**

```bash
$ rm src/lib/config.py
```

---

## Ejercicio 11: Renombrar un directorio

**Objetivo:** Renombrar el directorio `bin` a `scripts`.

**Solución:**

```bash
$ mv bin scripts
```

---

## Ejercicio 12: Copiar un directorio recursivamente

**Objetivo:** Copiar el directorio `doc` a `backup_doc`.

**Solución:**

```bash
$ cp -r doc backup_doc
```

---

## Ejercicio 13: Crear archivos con contenido

**Objetivo:** Crear un archivo `LICENSE` con el texto "MIT License".

**Solución:**

```bash
$ echo "MIT License" > LICENSE
```

---

## Ejercicio 14: Mover un directorio completo

**Objetivo:** Mover el directorio `scripts` dentro de `src`.

**Solución:**

```bash
$ mv scripts src/
```

---

## Ejercicio 15: Eliminar directorios vacíos

**Objetivo:** Eliminar el directorio `backup_doc` si está vacío.

**Solución:**

```bash
$ rmdir backup_doc
```

---

## Ejercicio 16: Eliminar directorios con contenido

**Objetivo:** Eliminar el directorio `backup_doc` y todo su contenido.

**Solución:**

```bash
$ rm -r backup_doc
```

---

## Ejercicio 17: Crear una estructura de directorios compleja

**Objetivo:** Crear la estructura `tests/unit` y `tests/integration` dentro de `proyecto`.

**Solución:**

```bash
$ mkdir -p tests/unit tests/integration
```

---
' '
## Ejercicio 18: Crear archivos numerados en un bucle

**Objetivo:** En `tests/unit`, crear los archivos `test1.py` a `test5.py`.

**Solución:**

```bash
$ for i in {1..5}; do touch tests/unit/test$i.py; done
```

---

## Ejercicio 19: Copiar y renombrar archivos

**Objetivo:** Copiar `LEEME.md` a `README.md` en el directorio `proyecto`.

**Solución:**

```bash
$ cp doc/LEEME.md README.md
```

---

## Ejercicio 20: Mover archivos según su extensión

**Objetivo:** Mover todos los archivos `.md` desde `proyecto` a `doc`.

**Solución:**

```bash
$ mv *.md doc/
```

---

## Ejercicio 21: Crear enlaces simbólicos

**Objetivo:** Crear un enlace simbólico llamado `run.sh` que apunte a `src/scripts/start.sh`.

**Solución:**

```bash
$ ln -s src/scripts/start.sh run.sh
```

---

## Ejercicio 22: Cambiar nombres de archivos en masa

**Objetivo:** En `tests/unit`, renombrar todos los archivos `test*.py` a `unit_test*.py`.

**Solución:**

```bash
$ cd tests/unit
$ for file in test*.py; do mv "$file" "unit_$file"; done
```

---

## Ejercicio 23: Mover directorios anidados

**Objetivo:** Mover `src/lib` dentro de `src/app`.

**Solución:**

```bash
$ mv src/lib src/app/
```

---

## Ejercicio 24: Eliminar archivos según patrón

**Objetivo:** Eliminar todos los archivos que comienzan con `unit_` en `tests/unit`.

**Solución:**

```bash
$ rm tests/unit/unit_*
```

---

## Ejercicio 25: Copiar archivos manteniendo la estructura de directorios

**Objetivo:** Copiar todos los archivos de `src` a `src_backup`, preservando la estructura.

**Solución:**

```bash
$ cp -r src src_backup
```

---

## Ejercicio 26: Crear directorios con fechas

**Objetivo:** Crear un directorio `backup_YYYYMMDD` con la fecha actual.

**Solución:**

```bash
$ mkdir backup_$(date +%Y%m%d)
```

---

## Ejercicio 27: Mover archivos según su tamaño

**Objetivo:** Mover archivos de más de 1MB desde `src` a `large_files`.

**Solución:**

```bash
$ mkdir large_files
$ find src -type f -size +1M -exec mv {} large_files/ \;
```

---

## Ejercicio 28: Crear archivos con números en su nombre

**Objetivo:** Crear archivos `data_1.txt` a `data_10.txt` en `data`.

**Solución:**

```bash
$ mkdir data
$ touch data/data_{1..10}.txt
```

---

## Ejercicio 29: Sincronizar directorios

**Objetivo:** Actualizar `src_backup` con los cambios de `src`.

**Solución:**

```bash
$ rsync -av src/ src_backup/
```

---

## Ejercicio 30: Organizar archivos por tipo

**Objetivo:** Mover archivos de `proyecto` a subdirectorios según su extensión.

**Solución:**

```bash
$ for file in proyecto/*.*; do
    ext="${file##*.}"
    mkdir -p proyecto/$ext
    mv "$file" proyecto/$ext/
done
```

---

## Ejercicio 31: Encontrar y eliminar archivos antiguos

**Objetivo:** Eliminar archivos en `logs` que no se han modificado en los últimos 30 días.

**Solución:**

```bash
$ find logs -type f -mtime +30 -exec rm {} \;
```

---

## Ejercicio 32: Copiar directorios excluyendo ciertos archivos

**Objetivo:** Copiar `src` a `src_clean` excluyendo archivos `*.log`.

**Solución:**

```bash
$ rsync -av --exclude='*.log' src/ src_clean/
```

---

## Ejercicio 33: Crear directorios desde una lista

**Objetivo:** Crear directorios listados en el archivo `dirs.txt`.

Contenido de `dirs.txt`:

```
archive
backup
reports
```

**Solución:**

```bash
$ while read dir; do mkdir "$dir"; done < dirs.txt
```

---

## Ejercicio 34: Renombrar archivos cambiando extensiones

**Objetivo:** Cambiar la extensión de todos los archivos `.txt` a `.bak` en `data`.

**Solución:**

```bash
$ for file in data/*.txt; do mv "$file" "${file%.txt}.bak"; done
```

---

## Ejercicio 35: Mover archivos a directorios basados en su nombre

**Objetivo:** Mover archivos que comienzan con `data_` a `data_files`.

**Solución:**

```bash
$ mkdir data_files
$ mv data/data_* data_files/
```

---

## Ejercicio 36: Crear enlaces duros para archivos importantes

**Objetivo:** Crear un enlace duro de `LICENSE` en `doc/LICENSE_COPY`.

**Solución:**

```bash
$ ln LICENSE doc/LICENSE_COPY
```

---

## Ejercicio 37: Organizar archivos por fecha

**Objetivo:** En `reports`, mover archivos a subdirectorios por año.

**Solución:**

```bash
$ cd reports
$ for file in *; do
    year=$(date -r "$file" +%Y)
    mkdir -p "$year"
    mv "$file" "$year/"
done
```

---

## Ejercicio 38: Borrar archivos excepto los más recientes

**Objetivo:** En `backups`, conservar solo los últimos 5 archivos.

**Solución:**

```bash
$ cd backups
$ ls -t | sed -e '1,5d' | xargs rm
```

---

## Ejercicio 39: Generar archivos desde una lista de nombres

**Objetivo:** Crear archivos con nombres listados en `users.txt`.

**Solución:**

```bash
$ while read user; do touch "${user}_profile.txt"; done < users.txt
```

---

## Ejercicio 40: Mover archivos según su contenido

**Objetivo:** Mover archivos que contienen la palabra "error" a `error_logs`.

**Solución:**

```bash
$ mkdir error_logs
$ grep -rl "error" logs/ | xargs mv -t error_logs/
```

---

## Ejercicio 41: Crear un directorio protegido

**Objetivo:** Crear `secure_data` con acceso solo para el usuario actual.

**Solución:**

```bash
$ mkdir secure_data
$ chmod 700 secure_data
```

---

## Ejercicio 42: Encontrar y copiar archivos específicos

**Objetivo:** Copiar archivos modificados en las últimas 24 horas a `recent_changes`.

**Solución:**

```bash
$ mkdir recent_changes
$ find . -type f -mtime -1 -exec cp {} recent_changes/ \;
```

---

## Ejercicio 43: Cambiar permisos de archivos

**Objetivo:** Hacer que todos los archivos en `scripts` sean ejecutables.

**Solución:**

```bash
$ chmod +x scripts/*
```

---

## Ejercicio 44: Crear un archivo con la lista de directorios

**Objetivo:** Generar `directories.txt` con todos los directorios en `proyecto`.

**Solución:**

```bash
$ find proyecto -type d > directories.txt
```

---

## Ejercicio 45: Renombrar archivos a minúsculas

**Objetivo:** Convertir todos los nombres de archivos en `data_files` a minúsculas.

**Solución:**

```bash
$ cd data_files
$ for file in *; do mv "$file" "$(echo $file | tr 'A-Z' 'a-z')"; done
```

---

## Ejercicio 46: Eliminar archivos temporales de compilación

**Objetivo:** En `src`, eliminar todos los archivos `*.o` y `*.tmp`.

**Solución:**

```bash
$ find src -type f \( -name "*.o" -o -name "*.tmp" \) -exec rm {} \;
```

---

## Ejercicio 47: Mover archivos modificados por un usuario específico

**Objetivo:** Mover archivos modificados por `usuario` a `user_changes`.

**Solución:**

```bash
$ mkdir user_changes
$ find . -type f -user usuario -exec mv {} user_changes/ \;
```

---

## Ejercicio 48: Crear backups incrementales

**Objetivo:** Copiar solo archivos nuevos o modificados desde `proyecto` a `incremental_backup`.

**Solución:**

```bash
$ rsync -av --ignore-existing proyecto/ incremental_backup/
```

---

## Ejercicio 49: Comparar dos directorios

**Objetivo:** Mostrar diferencias entre `src` y `src_backup`.

**Solución:**

```bash
$ diff -qr src/ src_backup/
```

---

## Ejercicio 50: Realizar una copia de seguridad con fecha y hora

**Objetivo:** Copiar `proyecto` a `backup_YYYYMMDD_HHMMSS`.

**Solución:**

```bash
$ cp -r proyecto backup_$(date +%Y%m%d_%H%M%S)
```

