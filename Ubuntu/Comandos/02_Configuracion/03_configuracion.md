### Unidad Didáctica: Configuración del Entorno y Uso de Variables en Archivos de Configuración de Bash

#### Objetivo
Explorar el uso de variables del sistema, junto con comillas y otros mecanismos, para personalizar el entorno de Bash mediante archivos de configuración como `.bashrc`, `.profile`, `/etc/profile`, entre otros.

---

### Archivos de Configuración del Sistema y su Propósito en Bash

Bash permite configurar el entorno a través de varios archivos que se ejecutan al iniciar una sesión. Estos archivos configuran variables de entorno, personalizan el prompt, definen alias y mucho más. Los archivos de configuración principales en Bash incluyen:

1. **`/etc/profile`**: Configuración global de entorno para todas las sesiones de usuario.
2. **`~/.profile`**: Configuración de entorno específica del usuario que se ejecuta al iniciar sesión.
3. **`~/.bashrc`**: Configuración de la shell interactiva que se carga cada vez que se abre una terminal.
4. **`~/.bash_profile`**: Se ejecuta en sesiones de login, común en algunas distribuciones que separan `.bashrc` para sesiones no-login.

---

### Variables de Entorno y Archivos de Configuración

Las **variables de entorno** permiten al usuario y al sistema almacenar información importante para las sesiones de trabajo, como el nombre de usuario (`$USER`), el directorio de inicio (`$HOME`), y la ruta de búsqueda de ejecutables (`$PATH`).

#### Variables Comunes en Archivos de Configuración

| Variable  | Descripción                                                |
|-----------|------------------------------------------------------------|
| `$USER`   | Nombre del usuario.                                        |
| `$HOME`   | Directorio principal del usuario.                          |
| `$PATH`   | Rutas donde el sistema busca ejecutables.                  |
| `$SHELL`  | Shell predeterminada del usuario.                          |
| `$PS1`    | Prompt de la línea de comandos en Bash.                    |
| `$PWD`    | Directorio actual.                                         |

Estas variables se pueden personalizar y expandir en los archivos de configuración.

---

### 1. Personalización en `/etc/profile`

El archivo `/etc/profile` establece configuraciones de entorno globales para todos los usuarios. Aquí se suelen configurar variables globales, como `$PATH`, que puede incluir rutas específicas necesarias para todos los usuarios del sistema.

#### Ejemplo: Añadir Rutas al `$PATH`

```bash
export PATH="$PATH:/usr/local/custom_bin"
```

Este código añade `/usr/local/custom_bin` al `$PATH` global, permitiendo a todos los usuarios ejecutar programas en esta ruta. Las comillas dobles (`"..."`) permiten la expansión de `$PATH`.

---

### 2. Configuración de Variables en `~/.profile`

El archivo `~/.profile` es específico para cada usuario y se ejecuta al iniciar sesión. Aquí puedes definir variables de usuario y personalizar el entorno de trabajo.

#### Ejemplo: Configurar un Alias para Sesiones de Usuario

```bash
export EDITOR="nano"
```

Con esta línea, se configura el editor de texto predeterminado como `nano`. Al usar comillas dobles, evitamos problemas con caracteres especiales y mantenemos la flexibilidad de la expansión.

#### Ejemplo: Mostrar un Mensaje de Bienvenida con Variables de Entorno

```bash
echo "Bienvenido, $USER! Fecha: $(date +%Y-%m-%d)"
```

Cada vez que inicies sesión, aparecerá el mensaje de bienvenida con el nombre del usuario y la fecha actual.

---

### 3. Personalización del Prompt en `~/.bashrc`

El archivo `~/.bashrc` se ejecuta cada vez que se abre una terminal interactiva. Aquí se configura el **prompt de la línea de comandos** y otros ajustes que hacen más productiva la sesión de trabajo.

#### Ejemplo: Configuración de `$PS1` para el Prompt

```bash
export PS1="[\u@\h \W]\$ "
```

- `\u`: Nombre de usuario.
- `\h`: Nombre del host.
- `\W`: Nombre del directorio actual.

**Salida**:
```
[user@hostname directory]$
```

#### Ejemplo: Añadir Colores al Prompt

```bash
export PS1="\[\033[01;34m\]\u@\h:\w\$ \[\033[00m\]"
```

Este prompt muestra el nombre de usuario, host y directorio actual en color azul. Aquí, usamos `\` para escapar los caracteres de color y permitir que sean interpretados correctamente en Bash.

---

### 4. Configuración Avanzada con `~/.bash_profile`

En algunas distribuciones, `.bash_profile` se usa en lugar de `.profile` para iniciar configuraciones de login. Es útil para definir alias y otras personalizaciones.

#### Ejemplo: Definir un Alias en `.bash_profile`

```bash
alias ll='ls -la'
```

Cada vez que abras una sesión de login, `ll` ejecutará `ls -la`, que lista los archivos en formato largo.

#### Ejemplo: Establecer Variables y Comandos en `.bash_profile`

```bash
export PATH="$PATH:$HOME/bin"
echo "Directorio bin del usuario añadido al PATH."
```

Aquí, el directorio `bin` del usuario se añade al `$PATH`, y se muestra un mensaje confirmando la acción.

---

### 5. Configuración en el Archivo `.bash_logout`

Este archivo se ejecuta al cerrar una sesión de Bash y permite realizar tareas de limpieza o registro de actividad.

#### Ejemplo: Mostrar un Mensaje al Cerrar la Sesión

```bash
echo "Hasta luego, $USER. ¡Vuelve pronto!"
```

Este mensaje aparecerá cada vez que se cierre la terminal, despidiendo al usuario.

---

### Ejemplos Combinados: Configuraciones Complejas

#### Ejemplo: Configuración de Variables, Alias y Prompt en `.bashrc`

```bash
# Definir variables personalizadas
export PATH="$PATH:$HOME/scripts"
export EDITOR="vim"

# Alias para facilitar el uso de comandos
alias update="sudo apt update && sudo apt upgrade"
alias gs="git status"

# Configuración del prompt con fecha y hora
export PS1="[\u@\h \W] \d \t > "
```

Este ejemplo personaliza el entorno de trabajo con variables de usuario (`$EDITOR`), añade rutas al `$PATH`, define alias útiles y ajusta el prompt para mostrar la fecha y hora.

#### Ejemplo: Configuración de Variables Globales en `/etc/profile`

```bash
export JAVA_HOME="/usr/lib/jvm/java-11-openjdk-amd64"
export PATH="$PATH:$JAVA_HOME/bin"
```

Estas configuraciones permiten que todos los usuarios tengan acceso a Java sin necesidad de definir el `JAVA_HOME` y el `$PATH` manualmente.

---

### Resumen de Archivos de Configuración y su Uso

| Archivo           | Descripción                                                         | Ejecución                       |
|-------------------|---------------------------------------------------------------------|---------------------------------|
| `/etc/profile`    | Configuración global para todas las sesiones de usuario.            | Al iniciar sesión.              |
| `~/.profile`      | Configuración de entorno específica del usuario.                    | Al iniciar sesión de usuario.   |
| `~/.bashrc`       | Configuración para shells interactivas, carga variables y alias.    | Al abrir una nueva terminal.    |
| `~/.bash_profile` | Alternativa a `.profile` en algunas distribuciones para login.      | Al iniciar sesión de usuario.   |
| `~/.bash_logout`  | Configuración para limpiar o registrar al cerrar la sesión.         | Al cerrar la terminal.          |

---

### Conclusión
Los archivos de configuración en Bash permiten personalizar el entorno de trabajo para cada usuario o de forma global. Con el uso adecuado de variables del sistema, alias y comandos, es posible optimizar la productividad y adaptarse a las necesidades específicas de cada sesión. Practicar la configuración en estos archivos proporciona un mayor control sobre el entorno y facilita una experiencia de usuario mejorada en Bash.