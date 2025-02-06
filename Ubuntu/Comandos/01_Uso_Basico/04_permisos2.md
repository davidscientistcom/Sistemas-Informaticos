- Se crearán dos grupos: `informaticos` y `gerencia`.
- Se crearán usuarios que pertenezcan a cada uno de estos grupos.
- Se generará una estructura de directorios en la que:
  - La carpeta de `informaticos` contendrá su propia información.
  - La carpeta de `gerencia` contendrá, entre otros, un subdirectorio `documentacion_publica`.
- Se configurará la seguridad de manera que:
  - Los miembros del grupo `gerencia` **puedan acceder a todo** lo que haya en el directorio de `informaticos`.
  - Los miembros del grupo `informaticos` **solo puedan acceder** a la carpeta `documentacion_publica` que se encuentra dentro del directorio de `gerencia`.


## Ejercicio 1: Creación de Grupos y Usuarios

### Paso 1.1: Crear los Grupos

Utilizaremos el comando `groupadd` para crear los grupos `informaticos` y `gerencia`.

```bash
# Crear grupo informaticos
sudo groupadd informaticos

# Crear grupo gerencia
sudo groupadd gerencia
```

> **Explicación:**  
> - `groupadd`: Permite crear un grupo en el sistema.  
> - Es fundamental que ambos grupos se creen antes de asignar usuarios a ellos.

### Paso 1.2: Crear Usuarios y Asignarlos a Grupos

Crearemos algunos usuarios de ejemplo para cada grupo. Se recomienda usar nombres representativos. Aquí creamos dos usuarios para cada grupo.

```bash
# Crear usuarios para el grupo informaticos
sudo useradd -m -G informaticos usuario1
sudo useradd -m -G informaticos usuario2

# Crear usuarios para el grupo gerencia
sudo useradd -m -G gerencia gerente1
sudo useradd -m -G gerencia gerente2
```

> **Explicación:**  
> - `useradd`: Comando para crear un usuario.  
> - La opción `-m` crea el directorio home del usuario automáticamente.  
> - La opción `-G` añade al usuario al grupo especificado.  
> - Si se requiere que el usuario pertenezca a grupos adicionales (por ejemplo, un usuario que pertenece a ambos grupos), se pueden listar separados por comas.

> **Nota:** En sistemas reales, es recomendable asignar contraseñas a estos usuarios utilizando `passwd usuarioX`.



## Ejercicio 2: Creación de la Estructura de Directorios

Se creará una estructura de directorios que simula la división de áreas en el sistema.

### Paso 2.1: Crear Directorios Base

Imaginemos que en la raíz (`/`) o en un directorio de pruebas (por ejemplo, `/home/ejercicios`) vamos a crear dos directorios principales: uno para `informaticos` y otro para `gerencia`.

```bash
# Crear directorio base para el ejercicio (opcional, para mantener el entorno limpio)
sudo mkdir -p /home/ejercicios

# Crear directorio para informaticos
sudo mkdir /home/ejercicios/informaticos

# Crear directorio para gerencia
sudo mkdir /home/ejercicios/gerencia
```

### Paso 2.2: Crear el Subdirectorio documentacion_publica en gerencia

```bash
# Crear subdirectorio dentro de gerencia
sudo mkdir /home/ejercicios/gerencia/documentacion_publica
```

> **Explicación:**  
> Se utiliza `mkdir` para crear directorios. La opción `-p` en el primer comando garantiza que se cree toda la ruta en caso de que no exista.



## Ejercicio 3: Asignación de Propietarios y Grupos con chown

Ahora asignaremos la propiedad y los grupos correspondientes a cada directorio.

### Paso 3.1: Asignar Directorio de Informaticos

Queremos que el directorio de `informaticos` pertenezca al grupo `informaticos`.

```bash
# Cambiar propietario a root y grupo a informaticos en el directorio informaticos
sudo chown root:informaticos /home/ejercicios/informaticos
```

> **Explicación:**  
> - `chown` cambia el propietario y el grupo de un archivo o directorio.  
> - En este caso, se deja a `root` como propietario (puedes cambiarlo a otro usuario si se requiere) y se asigna el grupo `informaticos`.

### Paso 3.2: Asignar Directorio de Gerencia

Queremos que el directorio `gerencia` pertenezca al grupo `gerencia`.

```bash
# Cambiar propietario a root y grupo a gerencia en el directorio gerencia
sudo chown root:gerencia /home/ejercicios/gerencia
```

### Paso 3.3: Asignar Subdirectorio documentacion_publica

Este subdirectorio debe ser accesible para ambos grupos de alguna forma, pero se le asignará inicialmente al grupo `gerencia` y luego configuraremos permisos.

```bash
# Cambiar propietario a root y grupo a gerencia en el subdirectorio documentacion_publica
sudo chown root:gerencia /home/ejercicios/gerencia/documentacion_publica
```

> **Consejo:**  
> La correcta asignación de propietarios y grupos es fundamental para que luego, mediante `chmod`, se puedan configurar los permisos deseados.



## Ejercicio 4: Configuración de Permisos con chmod

El objetivo es que:
- Los miembros del grupo `gerencia` puedan **acceder a todo** en `informaticos`.
- Los miembros del grupo `informaticos` puedan **acceder solamente a** `/home/ejercicios/gerencia/documentacion_publica`.

### Paso 4.1: Configurar Permisos para el Directorio de Informaticos

Queremos que todos los miembros del grupo `gerencia` tengan acceso a `informaticos`.  
Para ello, una estrategia es configurar los permisos del directorio de modo que cualquier miembro del grupo propietario (en este caso, `informaticos`) tenga acceso, y luego hacer que los usuarios de `gerencia` se añadan temporalmente al grupo `informaticos` o, mejor, utilizar ACLs (Listas de Control de Acceso).  
Sin embargo, dado que se busca practicar **chmod** y **chown**, abordaremos una solución que involucre la asignación de permisos básicos.

**Opción A: Utilizar permisos generales de lectura y ejecución**  
Si configuramos permisos 755, todos los usuarios (incluidos los de `gerencia`) podrán acceder al directorio. Pero esto no limita a los que no sean de `gerencia` a leer información interna, salvo que se apliquen restricciones adicionales.  
Podríamos pensar en separar los accesos, pero con **chmod** estándar la solución es limitada.  

**Opción B: Utilizar ACLs** (aunque se sale un poco de lo estricto uso de `chmod` y `chown`)  
Si queremos utilizar solo `chmod`, la opción más sencilla es dejar el directorio `informaticos` accesible a todos y luego controlar el acceso en `gerencia`.

Procederemos de la siguiente manera:

```bash
# Permitir lectura y ejecución (acceso) a todos en informaticos (rwxr-xr-x)
sudo chmod 755 /home/ejercicios/informaticos
```

> **Explicación:**  
> - `755` significa:  
>   - **7** (rwx) para el propietario (root),  
>   - **5** (r-x) para el grupo (informaticos),  
>   - **5** (r-x) para otros.  
>  
> De esta forma, cualquier usuario (incluyendo los de gerencia) puede acceder al directorio.  
>  
> **Nota:** Si se desea mayor control, se podrían usar ACLs (por ejemplo, con el comando `setfacl`), pero aquí nos centramos en el uso de `chmod`.

### Paso 4.2: Configurar Permisos para el Directorio de Gerencia

Queremos que:
- Todo el contenido en `/home/ejercicios/gerencia` solo sea accesible para usuarios de `gerencia`, excepto la carpeta `documentacion_publica` que además debe ser accesible para usuarios de `informaticos`.

Para el directorio `gerencia`, configuramos permisos restrictivos:

```bash
# Permitir solo al propietario (root) y al grupo gerencia acceso completo; denegar a otros
sudo chmod 770 /home/ejercicios/gerencia
```

> **Explicación:**  
> - `770` significa:  
>   - **7** (rwx) para el propietario,  
>   - **7** (rwx) para el grupo (gerencia),  
>   - **0** para otros (ningún permiso).  
>  
> Esto impide que usuarios que no pertenezcan a `gerencia` puedan acceder a cualquier cosa dentro de `/home/ejercicios/gerencia`, **excepto** aquello que se configure de manera especial en subdirectorios.

### Paso 4.3: Configurar Permisos para documentacion_publica

Queremos que:
- Los miembros del grupo `gerencia` tengan acceso completo.
- Los miembros del grupo `informaticos` puedan acceder a este directorio.

Una forma de hacerlo es cambiar el grupo del directorio a uno que incluya a ambos conjuntos de usuarios o asignar permisos “para otros” de manera que, aunque el directorio padre es 770, en este subdirectorio se permita el acceso a usuarios de `informaticos`.

**Solución usando permisos extendidos:**

1. Cambiar el grupo del subdirectorio a `gerencia` (ya lo hicimos) y luego:
2. Permitir acceso de lectura y ejecución a otros, o
3. Asignar permisos 775 para el subdirectorio. Sin embargo, con 775, “otros” (usuarios que no pertenezcan al grupo) podrán acceder.  
   
Para afinar la solución utilizando solo `chmod` y `chown`, una estrategia es:
- Crear el directorio con permisos `770` y luego cambiar el grupo propietario a uno que incluya a ambos grupos.  
- Pero dado que no podemos tener un usuario en dos grupos con `chown`, la solución clásica es utilizar ACLs.  
   
**Alternativa simplificada:**  
Permitir en `documentacion_publica` permisos de lectura y ejecución para “otros”, de forma que aunque el directorio padre (gerencia) es 770, si un usuario conoce la ruta, podría acceder al subdirectorio. Sin embargo, dado que el acceso al directorio padre está restringido, se requiere que el usuario de `informaticos` tenga un camino de acceso.  
   
**Procedimiento propuesto:**  
- Mover `documentacion_publica` a una ubicación común o modificar la estructura de permisos.  
   
**Solución recomendada sin ACLs:**  
Podemos dejar el directorio `gerencia` con permisos 750 o 770 para restringir el acceso, y en `documentacion_publica` permitir acceso “para otros” (modo 755 o 775). Esto supone que los usuarios de `informaticos` **no puedan** entrar al resto de `gerencia`, pero si conocen la ruta exacta, podrán acceder a `documentacion_publica`.

```bash
# Permitir lectura y ejecución a otros en documentacion_publica (rwxr-xr-x)
sudo chmod 755 /home/ejercicios/gerencia/documentacion_publica
```

> **Explicación:**  
> - `755` da:  
>   - **7** para el propietario (root),  
>   - **5** para el grupo (gerencia),  
>   - **5** para otros (esto permitirá que usuarios de informaticos, al no pertenecer a gerencia, puedan acceder a este directorio).  
>  
> **Importante:**  
> Para que un usuario de `informaticos` pueda acceder a `/home/ejercicios/gerencia/documentacion_publica`, deberá poder al menos ejecutar (acceder) el directorio `gerencia`.  
>  
> Dado que `/home/ejercicios/gerencia` tiene permisos 770, **solo** los usuarios pertenecientes a `gerencia` podrán acceder directamente.  
>  
> **Solución:**  
> Para que los usuarios de `informaticos` puedan acceder a `documentacion_publica`, se pueden aplicar dos estrategias:
>
> 1. **Utilizar ACLs:** Con el comando `setfacl` se puede conceder permisos adicionales en el directorio `gerencia` para el grupo `informaticos`, de modo que puedan atravesarlo y acceder a `documentacion_publica`.
>
> 2. **Crear un enlace simbólico o un acceso alternativo:** Ubicar `documentacion_publica` en un lugar accesible por ambos grupos.
>
> Procederemos con la **opción 1**, ya que es más didáctica.



## Ejercicio 5: Uso de ACLs para Permitir Acceso a documentacion_publica

### Paso 5.1: Conceder Acceso a Usuarios de Informaticos en el Directorio gerencia

Utilizaremos `setfacl` para permitir a los usuarios del grupo `informaticos` el permiso de ejecución (acceso) en el directorio `/home/ejercicios/gerencia`. Esto no les dará acceso al contenido del directorio (ya que tienen 770 en gerencia) pero sí les permitirá “atravesar” el directorio para llegar a `documentacion_publica`.

```bash
# Conceder permiso de ejecución (x) al grupo informaticos en el directorio gerencia
sudo setfacl -m g:informaticos:x /home/ejercicios/gerencia
```

> **Explicación:**  
> - `setfacl`: Comando para modificar las ACLs (Listas de Control de Acceso).  
> - `-m`: Modifica la ACL.  
> - `g:informaticos:x`: Al grupo `informaticos` se le concede el permiso de ejecución.  
>  
> Esto permite que un usuario de `informaticos` pueda "atravesar" el directorio `/home/ejercicios/gerencia` y, al tener permisos adecuados en `documentacion_publica`, acceder a él.

### Paso 5.2: Verificar las ACLs

Para asegurarnos de que la ACL se aplicó correctamente:

```bash
sudo getfacl /home/ejercicios/gerencia
```

Deberías ver una línea similar a:

```
# file: home/ejercicios/gerencia
# owner: root
# group: gerencia
user::rwx
group::rwx
group:informaticos:--x
mask::rwx
other::
```

### Paso 5.3: Configurar Permisos de documentacion_publica

Ya configuramos `chmod 755` en `documentacion_publica`, lo que permite el acceso a cualquier usuario que llegue hasta allí:

```bash
sudo chmod 755 /home/ejercicios/gerencia/documentacion_publica
```

Con esto, los usuarios de `informaticos` podrán:
- Acceder a `/home/ejercicios/gerencia` gracias a la ACL que les permite ejecución.
- Entrar en `documentacion_publica` gracias a los permisos 755.



## Resumen de la Estructura y Permisos

1. **Directorio `/home/ejercicios/informaticos`:**
   - Propietario: `root`
   - Grupo: `informaticos`
   - Permisos: `755` (todos pueden leer y ejecutar, pero solo el propietario puede escribir).

2. **Directorio `/home/ejercicios/gerencia`:**
   - Propietario: `root`
   - Grupo: `gerencia`
   - Permisos básicos: `770` (solo propietario y grupo gerencia pueden leer, escribir y ejecutar).
   - ACL adicional: Al grupo `informaticos` se le concede el permiso de ejecución (`x`) para poder atravesarlo.

3. **Subdirectorio `/home/ejercicios/gerencia/documentacion_publica`:**
   - Propietario: `root`
   - Grupo: `gerencia`
   - Permisos: `755` (permiten que, una vez dentro, cualquier usuario lea y ejecute; sin embargo, la restricción de acceso viene de la carpeta padre).



## Ejercicios Prácticos Adicionales

Para reforzar lo aprendido, se pueden plantear los siguientes ejercicios:

### Ejercicio 6: Creación y Prueba de Archivos

1. **Dentro de `/home/ejercicios/informaticos`, crea un archivo de prueba:**

   ```bash
   sudo touch /home/ejercicios/informaticos/info.txt
   sudo chmod 644 /home/ejercicios/informaticos/info.txt
   sudo chown root:informaticos /home/ejercicios/informaticos/info.txt
   ```

2. **Dentro de `/home/ejercicios/gerencia/documentacion_publica`, crea un archivo de prueba:**

   ```bash
   sudo touch /home/ejercicios/gerencia/documentacion_publica/nota.txt
   sudo chmod 644 /home/ejercicios/gerencia/documentacion_publica/nota.txt
   sudo chown root:gerencia /home/ejercicios/gerencia/documentacion_publica/nota.txt
   ```

3. **Prueba de acceso:**  
   - Inicia sesión como un usuario de `gerencia` (por ejemplo, `gerente1`) y verifica que puedas leer ambos archivos.  
   - Inicia sesión como un usuario de `informaticos` (por ejemplo, `usuario1`) y verifica que puedas acceder a `nota.txt` en `documentacion_publica` pero **no** a ningún contenido de `/home/ejercicios/gerencia` fuera de ese subdirectorio.

> **Sugerencia:**  
> Para cambiar de usuario y probar, puedes usar `su - usuario1` o abrir una nueva terminal.

### Ejercicio 7: Modificación de Permisos

1. **Cambia los permisos del directorio `informaticos` a `750` y analiza el comportamiento de los usuarios que no pertenecen al grupo `informaticos`.**

   ```bash
   sudo chmod 750 /home/ejercicios/informaticos
   ```

2. **Verifica con `ls -l` y comenta las diferencias en el acceso.**

> **Objetivo:**  
> Entender la importancia de los dígitos en los permisos y cómo afectan la visibilidad y el acceso a los contenidos de un directorio.

