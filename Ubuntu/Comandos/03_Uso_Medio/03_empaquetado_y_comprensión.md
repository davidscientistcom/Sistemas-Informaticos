# Guía Práctica de Compresión y Empaquetado en Linux

En Linux, existen diversas herramientas para la compresión y empaquetado de archivos. Aunque estos términos suelen usarse indistintamente, técnicamente son distintos: **empaquetar** significa agrupar múltiples archivos en uno solo sin compresión, mientras que **comprimir** implica reducir el tamaño de un archivo mediante algoritmos específicos. 

A continuación, se presentan los comandos `gzip`, `bzip2`, `tar`, `zip` y `unzip`, junto con sus opciones más importantes y ejemplos prácticos.

---

## Comando `gzip`

**Descripción:** `gzip` es una herramienta de compresión que utiliza el algoritmo DEFLATE para reducir el tamaño de un archivo. Se enfoca en comprimir archivos individuales y no empaqueta múltiples archivos en uno solo. El formato de archivo resultante es `.gz`.

**Principales Opciones:**
- `-d`: Descomprime un archivo `.gz`.
- `-k`: Conserva el archivo original después de la compresión.
- `-v`: Muestra información detallada del proceso de compresión o descompresión.

#### Ejercicio 1: Comprimir un Archivo con `gzip`
**Enunciado:** Crea un archivo de texto llamado `data.txt` dentro de `~/proyectos` y comprímelo usando `gzip`.

1. **Comandos:**
   ```bash
   touch ~/proyectos/data.txt
   gzip ~/proyectos/data.txt
   ```
   **Explicación:** `gzip` comprime el archivo y genera `data.txt.gz`. El archivo original se elimina por defecto, a menos que se use la opción `-k`.

#### Ejercicio 2: Comprimir y Mantener el Archivo Original
**Enunciado:** Usa la opción `-k` para comprimir el archivo `data.txt` sin eliminar el original.

1. **Comando:**
   ```bash
   gzip -k ~/proyectos/data.txt
   ```
   **Explicación:** La opción `-k` conserva el archivo `data.txt` después de comprimirlo.

#### Ejercicio 3: Descomprimir un Archivo `.gz`
**Enunciado:** Descomprime el archivo `data.txt.gz` para restaurar `data.txt`.

1. **Comando:**
   ```bash
   gzip -d ~/proyectos/data.txt.gz
   ```
   **Explicación:** La opción `-d` descomprime el archivo, recuperando `data.txt`.

---

## Comando `bzip2`

**Descripción:** `bzip2` también es un compresor de archivos, pero utiliza el algoritmo Burrows-Wheeler y la codificación Huffman. Suele ofrecer una mayor compresión que `gzip`, aunque el proceso es más lento. Los archivos comprimidos tienen la extensión `.bz2`.

**Principales Opciones:**
- `-d`: Descomprime un archivo `.bz2`.
- `-k`: Conserva el archivo original tras la compresión.
- `-v`: Muestra información detallada del proceso.

#### Ejercicio 4: Comprimir un Archivo con `bzip2`
**Enunciado:** Comprime `data.txt` con `bzip2` y conserva el archivo original.

1. **Comando:**
   ```bash
   bzip2 -k ~/proyectos/data.txt
   ```
   **Explicación:** Se genera `data.txt.bz2` y el archivo original `data.txt` se conserva.

#### Ejercicio 5: Descomprimir un Archivo `.bz2`
**Enunciado:** Descomprime el archivo `data.txt.bz2`.

1. **Comando:**
   ```bash
   bzip2 -d ~/proyectos/data.txt.bz2
   ```
   **Explicación:** `bzip2 -d` restaura el archivo original `data.txt`.

---

## Comando `tar`

**Descripción:** `tar` es una herramienta de empaquetado que permite agrupar múltiples archivos y directorios en un solo archivo, denominado tarball, con la extensión `.tar`. Aunque no comprime, puede usarse con `gzip` o `bzip2` para generar archivos comprimidos como `.tar.gz` o `.tar.bz2`.

**Principales Opciones:**
- `-c`: Crea un archivo.
- `-x`: Extrae un archivo.
- `-v`: Muestra detalles del proceso.
- `-f`: Especifica el nombre del archivo.
- `-z`: Comprime o descomprime con `gzip`.
- `-j`: Comprime o descomprime con `bzip2`.

#### Ejercicio 6: Empaquetar Directorios con `tar`
**Enunciado:** Agrupa `~/proyectos/practicas` en un archivo `practicas.tar`.

1. **Comando:**
   ```bash
   tar -cvf ~/proyectos/practicas.tar ~/proyectos/practicas
   ```
   **Explicación:** `tar -cvf` crea el archivo `practicas.tar` sin compresión.

#### Ejercicio 7: Comprimir un Tarball con `gzip`
**Enunciado:** Comprime `practicas.tar` usando `gzip` para crear `practicas.tar.gz`.

1. **Comando:**
   ```bash
   tar -czvf ~/proyectos/practicas.tar.gz ~/proyectos/practicas
   ```
   **Explicación:** La opción `-z` agrega compresión con `gzip`.

#### Ejercicio 8: Descomprimir y Extraer un Archivo `.tar.gz`
**Enunciado:** Extrae el contenido de `practicas.tar.gz` en `~/proyectos`.

1. **Comando:**
   ```bash
   tar -xzvf ~/proyectos/practicas.tar.gz -C ~/proyectos
   ```
   **Explicación:** `tar -xzvf` descomprime y extrae el archivo en el directorio especificado con `-C`.

---

## Comando `zip` y `unzip`

**Descripción:** `zip` y `unzip` son herramientas de compresión y descompresión para archivos `.zip`, un formato comúnmente usado en sistemas Windows. `zip` puede comprimir múltiples archivos y directorios en uno solo.

**Principales Opciones para `zip`:**
- `-r`: Comprime recursivamente todo el contenido de un directorio.
- `-v`: Muestra detalles de compresión.

**Principales Opciones para `unzip`:**
- `-l`: Lista los archivos en el archivo comprimido.
- `-d`: Especifica el directorio de destino para la extracción.

#### Ejercicio 9: Comprimir un Directorio con `zip`
**Enunciado:** Comprime la carpeta `practicas` en un archivo `practicas.zip`.

1. **Comando:**
   ```bash
   zip -r ~/proyectos/practicas.zip ~/proyectos/practicas
   ```
   **Explicación:** `-r` permite comprimir de forma recursiva, incluyendo subdirectorios.

#### Ejercicio 10: Listar el Contenido de un Archivo `.zip`
**Enunciado:** Lista los archivos contenidos en `practicas.zip`.

1. **Comando:**
   ```bash
   unzip -l ~/proyectos/practicas.zip
   ```
   **Explicación:** La opción `-l` muestra los archivos contenidos sin extraerlos.

#### Ejercicio 11: Extraer un Archivo `.zip`
**Enunciado:** Extrae `practicas.zip` en el directorio `~/proyectos`.

1. **Comando:**
   ```bash
   unzip ~/proyectos/practicas.zip -d ~/proyectos
   ```
   **Explicación:** La opción `-d` especifica el directorio donde se extraerá el contenido.
