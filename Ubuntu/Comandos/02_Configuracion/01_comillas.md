### Unidad Didáctica: Uso de Comillas y Escaping en Shell Bash

#### Objetivo
Aprender el funcionamiento y la utilidad de las comillas simples (`'`), dobles (`"`), y las comillas invertidas (\``\`) en el shell Bash. Se utilizarán ejemplos prácticos para entender cómo y cuándo usarlas, así como su impacto en la interpretación de comandos.

#### Introducción a las Comillas en Bash
En Bash, las comillas juegan un papel esencial en el manejo de texto y variables. Permiten al usuario controlar si ciertos caracteres deben ser interpretados literalmente o de forma especial.

- **Comillas Simples (`'... '`):** Usadas para evitar que el shell interprete cualquier carácter especial dentro de ellas.
- **Comillas Dobles (`"... "`):** Detienen la expansión de comodines, pero permiten la expansión de variables y la sustitución de comandos.
- **Comillas Invertidas (\``...`\`):** Utilizadas para la "sustitución de comandos", ejecutando un comando y reemplazándolo con su salida.

A continuación, exploraremos cada tipo de comillas con ejemplos y casos de uso.

---

### 1. Comillas Simples (`'...'`)

Las comillas simples encierran texto que no debe ser alterado. Cualquier carácter dentro de comillas simples será interpretado literalmente, sin que el shell trate de expandir variables, comodines u otros símbolos especiales.

#### Ejemplo 1: Texto Literal

```bash
echo 'Hoy es un buen día para aprender Bash!'
```
**Salida**:
```
Hoy es un buen día para aprender Bash!
```

#### Ejemplo 2: Evitando Expansión de Variables

```bash
nombre="Juan"
echo 'Hola, $nombre'
```
**Salida**:
```
Hola, $nombre
```

En este ejemplo, `$nombre` no se expande, sino que se muestra literalmente. Las comillas simples protegen cualquier carácter especial, incluyendo `$`.

---

### 2. Comillas Dobles (`"..."`)

Las comillas dobles permiten ciertas expansiones, como la de variables y la sustitución de comandos, pero evitan la expansión de comodines (por ejemplo, `*`, `?`, `[ ]`).

#### Ejemplo 1: Expansión de Variables

```bash
nombre="Juan"
echo "Hola, $nombre"
```
**Salida**:
```
Hola, Juan
```

Aquí, `$nombre` se expande a su valor porque está dentro de comillas dobles.

#### Ejemplo 2: Expansión de Comandos con `date`

```bash
echo "La fecha de hoy es: $(date)"
```
**Salida**:
```
La fecha de hoy es: [fecha actual]
```

**Alternativa con Comillas Invertidas**:
Otra forma de hacer la sustitución de comandos es usando comillas invertidas:

```bash
echo "La fecha de hoy es: `date`"
```

Ambos métodos (`$(comando)` o \``comando`\`) son válidos y equivalentes en Bash.

#### Ejemplo 3: Preservación de Comodines

```bash
echo "Este es un asterisco: *"
```
**Salida**:
```
Este es un asterisco: *
```

El asterisco (`*`) no se expande para mostrar archivos o directorios coincidentes, ya que está dentro de comillas dobles.

---

### 3. Comillas Invertidas (\``comando`\`)

Las comillas invertidas ejecutan un comando y sustituyen su valor en el lugar donde aparecen. Esto es conocido como **sustitución de comandos**.

#### Ejemplo 1: Obtener la Fecha Actual

```bash
echo "Hoy es: `date`"
```
**Salida**:
```
Hoy es: [fecha actual]
```

En este ejemplo, el comando `date` se ejecuta y su resultado reemplaza a \``date`\` en el comando.

#### Ejemplo 2: Sustitución en una Asignación de Variable

```bash
directorio=`pwd`
echo "Estás en el directorio: $directorio"
```
**Salida**:
```
Estás en el directorio: /ruta/al/directorio
```

Aquí, \``pwd`\` se sustituye con el directorio actual, que se asigna a la variable `directorio`.

---

### 4. Escaping con `\`

El **carácter de escape** (`\`) permite evitar la interpretación de un carácter específico. Esto es útil cuando solo se desea evitar la expansión de un carácter sin usar comillas.

#### Ejemplo 1: Escapar el Dólar

```bash
echo "Precio total: \$100"
```
**Salida**:
```
Precio total: $100
```

Aquí, `\$` evita que el shell interprete `$` como un inicio de variable.

#### Ejemplo 2: Escapando Comillas

```bash
echo "Este es un ejemplo de \"comillas dobles\" dentro de comillas dobles"
```
**Salida**:
```
Este es un ejemplo de "comillas dobles" dentro de comillas dobles
```

La barra invertida `\` permite incluir comillas dobles dentro de una cadena rodeada por comillas dobles.

---

### Resumen y Comparativa

| Tipo de Comillas | Características                                                                                       | Ejemplo                           | Salida Ejemplo                    |
|------------------|------------------------------------------------------------------------------------------------------|-----------------------------------|------------------------------------|
| **Simples** `'` | No permite ninguna expansión.                                                                         | `'Hola, $nombre'`                | `Hola, $nombre`                   |
| **Dobles** `"`  | Permite expansión de variables y sustitución de comandos, no permite comodines (`*`, `?`, `[ ]`).     | `"Hola, $nombre"`                | `Hola, Juan`                      |
| **Invertidas** \`` | Sustituyen el comando con su salida (equivalente a `$(comando)`).                                   | `` `date` ``                     | `[fecha actual]`                  |
| **Escaping** `\` | Evita la expansión de un solo carácter especial.                                                      | `echo "Precio: \$100"`           | `Precio: $100`                    |

---

### Ejercicios Prácticos

1. **Imprimir el valor de una variable literal**:
   - Crea una variable `mensaje="Hola!"` e imprime literalmente `$mensaje` sin expandirlo.
   - **Solución**:
     ```bash
     mensaje="Hola!"
     echo '$mensaje'
     ```

2. **Imprimir la lista de archivos en el directorio actual, mostrando el asterisco literalmente**:
   ```bash
   echo "Lista de archivos: *"
   ```

3. **Utilizar `date` para obtener y mostrar la fecha dentro de una oración**:
   ```bash
   echo "La fecha y hora actual es: $(date)"
   ```

4. **Combinar varias comillas**:
   - Muestra el mensaje: `"La variable $USER contiene tu nombre de usuario actual"`.
   - **Solución**:
     ```bash
     echo "\"La variable \$USER contiene tu nombre de usuario actual\""
     ```
