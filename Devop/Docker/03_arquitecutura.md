# 1. Recapitulación: Imágenes, Capas y Contenedores

Antes de entrar en detalles sobre la eliminación y la caché, recordemos algunos conceptos fundamentales:

- **Imagen:**  
  Una imagen es un conjunto de capas inmutables (generalmente de solo lectura) que, en conjunto, definen el sistema de archivos y la configuración para ejecutar una aplicación. Cada instrucción en un Dockerfile genera una capa que se apila para formar la imagen final.

- **Capa:**  
  Cada capa es el resultado de una instrucción (por ejemplo, `FROM`, `RUN`, `COPY`, etc.). Una vez creada, la capa es inmutable; es decir, si se requiere un cambio, se genera una nueva capa sobre la anterior.

- **Contenedor:**  
  Es una instancia en ejecución de una imagen. Al iniciarse, Docker crea una **capa de escritura** (o capa superior) que se añade a las capas inmutables de la imagen. Esta capa de escritura permite que el contenedor realice modificaciones sin afectar la imagen base.

- **Caché Interna:**  
  Durante el proceso de construcción de una imagen, Docker utiliza una caché que almacena las capas generadas. Si una instrucción del Dockerfile no ha cambiado (y sus entradas tampoco), Docker reutiliza la capa correspondiente para acelerar la construcción.


# 2. Eliminación de Contenedores e Imágenes: ¿Qué Sucede con las Capas y la Caché?

## 2.1. Eliminación de un Contenedor

### ¿Qué ocurre?

- **Capa de Escritura:**  
  Al eliminar un contenedor (por ejemplo, mediante `docker rm`), Docker borra la **capa de escritura** asociada a ese contenedor y elimina su metadata (registros, identificadores, etc.).  
  - **Importante:** Las capas de solo lectura que provienen de la imagen **no se ven afectadas**. Estas capas permanecen en el sistema y en la caché de Docker, ya que pueden ser utilizadas por otros contenedores o incluso en futuras construcciones.

### Ejemplo Práctico

Supongamos que creamos un contenedor a partir de la imagen `mi-imagen`:

```bash
# Crear un contenedor basado en la imagen "mi-imagen"
docker run -d --name contenedor_ejemplo mi-imagen

# Verificar que el contenedor está en ejecución
docker ps

# Eliminar el contenedor
docker rm -f contenedor_ejemplo
```

En este caso, al eliminar el contenedor:
- **Se elimina:** La capa de escritura única del contenedor y sus datos asociados.
- **Se conserva:** La imagen `mi-imagen` y todas sus capas inmutables, así como la caché interna que permitió su construcción.

## 2.2. Eliminación de una Imagen

### ¿Qué ocurre?

Cuando se elimina una imagen (por ejemplo, usando `docker rmi`), se deben considerar las dependencias:

- **Imagen y sus Capas:**  
  Al ejecutar `docker rmi <imagen>`, Docker intenta eliminar la referencia a la imagen y, si no existen contenedores que dependan de ella, las capas de la imagen se marcan para ser removidas.
  
- **Dependencia de Contenedores:**
  - **Contenedor Activo o Existente:**  
    Si existe un contenedor (incluso si está detenido) que fue creado a partir de esa imagen, Docker **no eliminará** completamente las capas subyacentes. Esto se debe a que el contenedor mantiene una referencia a esas capas para poder funcionar correctamente.  
    - **Caso Forzado:**  
      Con `docker rmi -f <imagen>` se puede forzar la eliminación de la imagen, pero los contenedores que ya fueron creados seguirán teniendo acceso a las capas almacenadas en su sistema de archivos. Es decir, el contenedor sigue funcionando aunque la imagen ya no figure en la lista de imágenes disponibles.
  - **Sin Contenedores Asociados:**  
    Si no hay ningún contenedor utilizando la imagen, Docker procede a eliminar la imagen y sus capas (si no están referenciadas por otra imagen). La caché interna relacionada a esas capas también se puede limpiar si ya no se utilizan en futuras construcciones.

### Ejemplo Práctico

Imagina el siguiente escenario:

```bash
# Listar imágenes disponibles
docker images

# Supongamos que "mi-imagen" está disponible y ya se creó un contenedor basado en ella:
docker run -d --name contenedor_test mi-imagen

# Intentar eliminar la imagen
docker rmi mi-imagen
```

- **Si existe el contenedor "contenedor_test":**  
  Docker devolverá un error indicando que la imagen está en uso.  
  Para forzar la eliminación, se podría usar:
  ```bash
  docker rmi -f mi-imagen
  ```
  Sin embargo, el contenedor "contenedor_test" seguirá funcionando, ya que posee en su sistema de archivos las capas de solo lectura necesarias.

- **Si no existen contenedores asociados:**  
  La imagen y sus capas se eliminarán, liberando espacio en disco.


# 3. ¿Por Qué Puedo Tener Contenedores Funcionando Habiendo Borrado la Imagen?

Este comportamiento puede resultar sorprendente, pero se debe a cómo Docker gestiona internamente las dependencias:

- **Referencia en el Momento de Creación:**  
  Cuando se crea un contenedor, Docker construye su sistema de archivos combinando:
  - Las capas **inmutables** de la imagen (leídas de la caché interna).
  - Una capa de **escritura** exclusiva para el contenedor.
  
  Esta combinación se consolida en el contenedor, el cual **mantiene una referencia interna** a las capas inmutables, independientemente de la existencia de la imagen en la lista de imágenes.

- **Mecanismo de Garbage Collection (Recolección de Basura):**  
  Docker posee un sistema que se encarga de eliminar las capas que ya no están referenciadas. Mientras exista al menos un contenedor que use dichas capas, Docker las considera necesarias y no las elimina.
  
- **Implicación Práctica:**  
  Esto significa que si eliminas la imagen, el contenedor sigue funcionando porque:
  - Su sistema de archivos ya se ha "concretado" a partir de las capas de la imagen.
  - La eliminación de la imagen solo afecta la posibilidad de crear **nuevos** contenedores a partir de esa imagen, pero no interfiere con los contenedores ya existentes.

### Ejemplo Ilustrativo

```bash
# Crear contenedor basado en "mi-imagen"
docker run -d --name contenedor_funcional mi-imagen

# Forzar la eliminación de la imagen
docker rmi -f mi-imagen

# Verificar que el contenedor sigue funcionando
docker ps
```

En este ejemplo, aunque la imagen "mi-imagen" ya no aparece en la lista de imágenes, el contenedor "contenedor_funcional" continúa operando sin problemas porque conserva las capas inmutables que necesitó para su sistema de archivos en el momento de su creación.

# 4. Relación de Caché, Capas, Contenedores e Imágenes: Consideraciones y Buenas Prácticas

La arquitectura de Docker, basada en capas y en un sistema de caché, ofrece grandes ventajas pero también implica algunas consideraciones importantes:

- **Reutilización de Capas en la Caché:**
  - Durante la construcción de imágenes, la reutilización de capas acelera el proceso y optimiza el uso de recursos.  
  - Cambiar una instrucción que está en medio del Dockerfile puede invalidar la caché para todas las capas subsiguientes. Por ello, es recomendable:
    - Colocar instrucciones que cambian poco (instalación de dependencias) al inicio.
    - Ubicar instrucciones con cambios frecuentes (copia del código fuente) al final.

- **Gestión de Dependencias:**
  - **Contenedores vs Imágenes:**  
    Los contenedores dependen de las capas inmutables de la imagen en el momento de su creación. La eliminación de una imagen no afecta a los contenedores ya creados.
  - **Limpieza Controlada:**  
    Utilizar comandos como `docker image prune` y `docker container prune` permite liberar espacio de forma controlada, eliminando capas y contenedores que ya no están referenciados.
  - **Forzar Eliminaciones:**  
    Forzar la eliminación de una imagen (con `-f`) puede dejar contenedores huérfanos en cuanto a la referencia visual de la imagen, pero no afecta su funcionamiento real.

- **Consideración Final sobre la Caché:**
  - La caché de construcción es independiente de la vida de los contenedores. Esto significa que, aun cuando se eliminen contenedores o imágenes, las capas que aún son útiles (o referenciadas) permanecen en el sistema, permitiendo reconstrucciones más rápidas en el futuro.
