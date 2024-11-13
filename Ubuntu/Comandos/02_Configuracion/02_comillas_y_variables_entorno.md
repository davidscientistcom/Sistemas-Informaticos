### Unidad Didáctica: Uso de Comillas y Variables de Sistema en Bash

#### Objetivo
Comprender cómo y cuándo usar comillas para manipular variables del sistema en Bash y explorar ejemplos con variables comunes. También identificaremos archivos de configuración clave donde se utilizan estas variables.

---

### Introducción a las Variables de Sistema y Archivos de Configuración en Bash

En Bash, existen variables de sistema (también conocidas como **variables de entorno**) que contienen información útil y relevante sobre el entorno de trabajo, como el nombre de usuario, el directorio actual, o la configuración de la shell. Además, estas variables son fundamentales en archivos de configuración, como `.bashrc` o `.profile`, donde se personalizan aspectos del entorno del usuario.

---

### 1. Tipos de Comillas en Bash (Repaso Breve)

- **Comillas Simples (`'...'`):** Evitan la expansión de variables y cualquier interpretación de caracteres especiales.
- **Comillas Dobles (`"..."`):** Permiten la expansión de variables y comandos, pero no de comodines.
- **Comillas Invertidas (\``...`\` o `$(...)`):** Usadas para ejecutar comandos dentro de otros comandos y reemplazarlos por sus salidas.

### 2. Variables del Sistema en Bash

Las variables del sistema en Bash suelen estar en **mayúsculas** y proporcionan detalles sobre el entorno de trabajo. Las más utilizadas incluyen:

- `$USER`: Nombre de usuario actual.
- `$HOME`: Directorio principal del usuario.
- `$PATH`: Rutas donde el sistema busca los ejecutables.
- `$PWD`: Directorio de trabajo actual.
- `$SHELL`: Ruta de la shell predeterminada.

Estas variables pueden definirse y configurarse en archivos de configuración del shell, como `.bashrc`, `.bash_profile`, o `.profile`.

---

### 3. Uso de Comillas con Variables del Sistema

Veamos cómo cada tipo de comilla afecta el uso de estas variables.

#### a. Comillas Simples (`'...'`)

Las comillas simples tratan todo su contenido como texto literal, evitando la expansión de variables del sistema.

##### Ejemplo 1: Evitar la Expansión de Variables del Sistema

```bash
echo 'El usuario actual es $USER y el directorio de inicio es $HOME'
```
**Salida**:
```
El usuario actual es $USER y el directorio de inicio es $HOME
```

Al no expandirse `$USER` y `$HOME`, la salida contiene los nombres de las variables literalmente.

---

#### b. Comillas Dobles (`"..."`)

Las comillas dobles permiten la expansión de variables, lo que resulta útil para mostrar información dinámica del entorno de trabajo.

##### Ejemplo 2: Mostrar Información de Variables del Sistema

```bash
echo "El usuario actual es $USER y el directorio de inicio es $HOME"
```
**Salida**:
```
El usuario actual es [nombre_del_usuario] y el directorio de inicio es [ruta_del_directorio_home]
```

En este caso, `$USER` y `$HOME` se expanden, mostrando el nombre del usuario y el directorio de inicio respectivamente.

---

#### c. Comillas Invertidas (\``...`\` o `$(...)`)

Las comillas invertidas y `$(...)` permiten ejecutar comandos dentro de otros comandos, lo que es útil para mostrar información adicional del sistema en tiempo real.

##### Ejemplo 3: Obtener y Mostrar el Directorio Actual

```bash
echo "Estás en el directorio: $(pwd)"
```
**Salida**:
```
Estás en el directorio: [ruta_actual]
```

Aquí, `$(pwd)` ejecuta el comando `pwd`, que muestra el directorio de trabajo actual.

---

### 4. Variables del Sistema en Archivos de Configuración

Es común usar variables de sistema en archivos como `.bashrc` o `.profile` para personalizar el entorno del usuario. A continuación, se muestra cómo estas variables pueden configurarse y personalizarse.

#### Archivo `.bashrc`

El archivo `.bashrc` se ejecuta cada vez que se abre una terminal interactiva en modo no-login. Aquí puedes configurar variables como el `PATH` o definir mensajes personalizados usando variables de entorno.

##### Ejemplo 4: Personalización del Prompt (PS1)

```bash
export PS1="Usuario: $USER en directorio: $PWD > "
```

Esta línea establece un prompt que muestra el nombre del usuario y el directorio actual cada vez que se abre la terminal. `$USER` y `$PWD` se expanden a su valor actual gracias a las comillas dobles.

#### Archivo `.profile`

El archivo `.profile` se ejecuta solo al iniciar sesión y también puede incluir configuraciones de variables.

##### Ejemplo 5: Configurar el PATH

```bash
export PATH="$PATH:/ruta/adicional"
```

Este comando añade una ruta adicional a la variable `$PATH` usando comillas dobles para permitir la expansión de la variable actual `$PATH`.

---

### 5. Ejemplos Prácticos de Uso de Comillas con Variables de Sistema

A continuación, se presentan algunos ejemplos prácticos donde se usan variables de entorno junto con diferentes tipos de comillas.

#### Ejemplo 6: Mensaje de Bienvenida Personalizado en `.bashrc`

Dentro del archivo `.bashrc`, añade el siguiente mensaje para que aparezca al abrir una terminal:

```bash
echo "Bienvenido, $USER! Tu directorio de trabajo es $PWD"
```

Cada vez que abras una nueva terminal, verás un mensaje con tu nombre de usuario y el directorio actual. Al estar entre comillas dobles, `$USER` y `$PWD` se expanden.

#### Ejemplo 7: Visualizar el PATH Literalmente

Si deseas ver el contenido literal de `$PATH` sin expansión:

```bash
echo '$PATH'
```
**Salida**:
```
$PATH
```

Las comillas simples evitan que `$PATH` se expanda, mostrándolo literalmente.

#### Ejemplo 8: Escapar el Dólar en Mensajes

Si deseas mostrar el símbolo `$` sin que el shell intente expandirlo, puedes usar `\`:

```bash
echo "Tu saldo es \$100"
```
**Salida**:
```
Tu saldo es $100
```

#### Ejemplo 9: Ejecución de Comandos en el Prompt

En `.bashrc` o `.profile`, puedes hacer que el prompt muestre la fecha y hora actuales:

```bash
export PS1="Fecha: $(date +%Y-%m-%d) > "
```

Este comando ejecuta `date` y muestra la fecha actual en el prompt.

---

### Resumen de Uso de Comillas con Variables de Sistema

| Tipo de Comillas | Ejemplo de Uso                                  | Expansión de Variables | Expansión de Comandos |
|------------------|-------------------------------------------------|------------------------|------------------------|
| **Simples** `'` | `'El usuario es $USER'`                          | No                     | No                     |
| **Dobles** `"`  | `"El usuario es $USER y el shell es $SHELL"`     | Sí                     | No                     |
| **Invertidas** \`` | `` echo "El directorio es `pwd`" ``           | No                     | Sí                     |
| **Escaping** `\` | `echo "Tu saldo es \$100"`                      | N/A                    | N/A                    |
