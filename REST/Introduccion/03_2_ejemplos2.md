Para interactuar con una **API REST** desde la línea de comandos, uno de los comandos más útiles y versátiles es **`curl`**. `curl` te permite realizar solicitudes HTTP a servidores y recibir respuestas, lo que es invaluable para probar y trabajar con APIs. En esta sección, explicaremos cómo utilizar `curl` para hacer peticiones GET, POST, PUT, PATCH y DELETE a una API REST pública, brindando varios ejemplos prácticos. 

## 1. Introducción a cURL

`curl` (Client URL) es una herramienta de línea de comandos que se utiliza para transferir datos desde o hacia un servidor, usando distintos protocolos. Para las APIs REST, `curl` se utiliza generalmente con el protocolo HTTP/S.

La sintaxis básica de `curl` es:

```bash
curl [opciones] [URL]
```

**Opciones comunes**:

- `-X`: Especifica el método HTTP (GET, POST, PUT, PATCH, DELETE) a utilizar.
- `-H`: Añade un encabezado HTTP a la solicitud.
- `-d`: Envía datos en la solicitud HTTP, generalmente en formato JSON para APIs REST, usado junto con POST, PUT o PATCH.
- `-i`: Muestra los encabezados de la respuesta junto con el cuerpo.
- `-s`: Silencia la barra de progreso, mostrando solo la información esencial.
- `-v`: Muestra la información detallada (verbose), incluyendo la solicitud y respuesta HTTP completas.
- `-w`: Utilizado para formatear la información de salida, puede mostrar detalles como el tiempo de respuesta, código de estado, etc.

Al combinar estas opciones, podemos crear peticiones HTTP precisas y ver cómo el servidor responde.

## 2. Ejemplo de API Pública: JSONPlaceholder

Para nuestros ejemplos, utilizaremos la API pública [**JSONPlaceholder**](https://jsonplaceholder.typicode.com/). JSONPlaceholder es una API REST gratuita para probar y prototipar, que simula los métodos y respuestas típicos de una API REST, sin la necesidad de implementar nuestro propio servidor.

### Recursos Disponibles en JSONPlaceholder

- **Posts**: `/posts`
- **Comments**: `/comments`
- **Albums**: `/albums`
- **Photos**: `/photos`
- **Todos**: `/todos`
- **Users**: `/users`

Cada uno de estos recursos puede ser accedido y modificado usando métodos HTTP estándar.

## 3. Realizando Peticiones GET con cURL

Las solicitudes **GET** se utilizan para obtener recursos del servidor sin modificar su estado.

### 3.1. Obtener Todos los Posts

Para obtener la lista completa de "posts" (entradas o publicaciones) en JSONPlaceholder, utilizamos:

```bash
curl -X GET https://jsonplaceholder.typicode.com/posts
```

Esta solicitud devuelve un array JSON con todos los posts. Dado que GET es el método predeterminado de `curl`, podemos omitir `-X GET` y simplemente hacer:

```bash
curl https://jsonplaceholder.typicode.com/posts
```

#### Ejemplo de Salida

```json
[
  {
    "userId": 1,
    "id": 1,
    "title": "sunt aut facere repellat provident ...",
    "body": "quia et suscipit suscipit recusandae ..."
  },
  {
    "userId": 1,
    "id": 2,
    "title": "qui est esse",
    "body": "est rerum tempore vitaese..."
  },
  ...
]
```

### 3.2. Obtener un Recurso Específico

Para obtener un recurso individual, como un post con ID 1:

```bash
curl https://jsonplaceholder.typicode.com/posts/1
```

#### Ejemplo de Salida

```json
{
  "userId": 1,
  "id": 1,
  "title": "sunt aut facere repellat provident ...",
  "body": "quia et suscipit suscipit recusandae ..."
}
```

### 3.3. Obtener con Parámetros y Filtrado (Opcional)

Algunas APIs permiten pasar parámetros de consulta para filtrar resultados. JSONPlaceholder soporta, por ejemplo, filtrar posts por `userId`:

```bash
curl https://jsonplaceholder.typicode.com/posts?userId=1
```

Esto devolverá todos los posts creados por el usuario con `userId` = 1.

## 4. Realizando Peticiones POST con cURL

Las solicitudes **POST** se utilizan para crear nuevos recursos en el servidor. Debemos enviar datos en el cuerpo de la petición, usualmente en formato JSON.

Para enviar datos JSON, utilizamos la opción `-d` y el encabezado `Content-Type: application/json`.

### 4.1. Crear un Nuevo Post

Para crear un nuevo post en JSONPlaceholder:

```bash
curl -X POST \
     -H "Content-Type: application/json" \
     -d '{"title": "Mi nuevo post", "body": "Contenido del post", "userId": 1}' \
     https://jsonplaceholder.typicode.com/posts
```

**Explicación**:
- `-X POST` indica el método.
- `-H "Content-Type: application/json"` establece el encabezado indicando que el contenido de la petición es JSON.
- `-d '{"title": "Mi nuevo post", "body": "Contenido del post", "userId": 1}'` envía el JSON con los campos requeridos por el API.

#### Ejemplo de Salida

```json
{
  "title": "Mi nuevo post",
  "body": "Contenido del post",
  "userId": 1,
  "id": 101
}
```

Observa que el API asigna un `id` al nuevo post creado. En JSONPlaceholder, la creación real no persiste, pero este ejemplo muestra cómo funcionaría en una API real.

## 5. Realizando Peticiones PUT con cURL

Las solicitudes **PUT** se utilizan para actualizar completamente un recurso existente. Esto implica que se deben enviar todos los campos del recurso, incluyendo aquellos que no cambian, ya que se reemplaza la entidad completa.

### 5.1. Actualizar un Post Existente

Para actualizar el post con ID 1:

```bash
curl -X PUT \
     -H "Content-Type: application/json" \
     -d '{"id": 1, "title": "Título actualizado", "body": "Contenido actualizado", "userId": 1}' \
     https://jsonplaceholder.typicode.com/posts/1
```

**Explicación**:
- `-X PUT` indica el método.
- Se envía el objeto completo, incluso el campo `id`.

#### Ejemplo de Salida

```json
{
  "id": 1,
  "title": "Título actualizado",
  "body": "Contenido actualizado",
  "userId": 1
}
```

Esto demuestra cómo sería actualizar un recurso. En JSONPlaceholder, la respuesta simula la actualización sin realizar cambios persistentes en el servidor.

## 6. Realizando Peticiones PATCH con cURL

Las solicitudes **PATCH** se utilizan para actualizar parcialmente un recurso existente. Esto significa que solamente enviamos los campos que cambian, dejando intactos el resto.

### 6.1. Actualizar Parcialmente un Post Existente

Para actualizar solo el título de un post con ID 1:

```bash
curl -X PATCH \
     -H "Content-Type: application/json" \
     -d '{"title": "Nuevo título parcial"}' \
     https://jsonplaceholder.typicode.com/posts/1
```

**Explicación**:
- `-X PATCH` indica el método.
- Solo enviamos el campo que queremos actualizar, en este caso `title`.

#### Ejemplo de Salida

```json
{
  "id": 1,
  "title": "Nuevo título parcial",
  "body": "Contenido actualizado",
  "userId": 1
}
```

La salida muestra que solo el campo `title` se ha actualizado, y los demás campos permanecen iguales (según la simulación de JSONPlaceholder). En una API real, la respuesta confirmaría la modificación de los campos enviados.

## 7. Realizando Peticiones DELETE con cURL

Las solicitudes **DELETE** se utilizan para eliminar un recurso existente en el servidor.

### 7.1. Eliminar un Post Existente

Para eliminar el post con ID 1:

```bash
curl -X DELETE https://jsonplaceholder.typicode.com/posts/1
```

**Explicación**:
- `-X DELETE` indica el método HTTP para eliminar el recurso.

#### Ejemplo de Salida

La respuesta exitosa para una petición DELETE generalmente es un código de estado `200` o `204` sin contenido en el cuerpo (o con algún mensaje en JSON). JSONPlaceholder responde con:

```json
{}
```

Esto indica que el recurso se eliminó exitosamente (simulado en este caso).

## 8. Añadiendo Encabezados HTTP y Autenticación

Al trabajar con APIs REST, a menudo necesitamos enviar encabezados HTTP adicionales para autenticación, manejo de tokens, etc.

### 8.1. Encabezados de Autorización

Muchas APIs requieren que se envíe un encabezado `Authorization`. Por ejemplo:

```bash
curl -X GET \
     -H "Authorization: Bearer <mi_token>" \
     https://api.miapp.com/usuarios
```

Reemplaza `<mi_token>` con el token o credenciales necesarias. 

### 8.2. Otros Encabezados

Podemos enviar múltiples encabezados usando varias opciones `-H`. Por ejemplo, para enviar una solicitud con `Authorization` y `Content-Type`:

```bash
curl -X POST \
     -H "Authorization: Bearer <mi_token>" \
     -H "Content-Type: application/json" \
     -d '{"titulo": "nuevo"}' \
     https://api.miapp.com/posts
```

## 9. Manejo de Respuestas y Códigos de Estado

### 9.1. Mostrar Encabezados de Respuesta

Para ver los encabezados de respuesta junto con el cuerpo, usamos `-i`:

```bash
curl -i https://jsonplaceholder.typicode.com/posts/1
```

#### Ejemplo de Salida

```http
HTTP/1.1 200 OK
Content-Type: application/json; charset=utf-8
...
```

JSONPlaceholder devolverá encabezados HTTP relevantes, como el código de estado HTTP (`200 OK` en este caso).

### 9.2. Verificar el Código de Estado HTTP

Para ver únicamente el código de estado HTTP de la respuesta, podemos utilizar `-o /dev/null` (descartar la salida) y `-w "%{http_code}\n"` (imprimir solo el código):

```bash
curl -o /dev/null -s -w "%{http_code}\n" https://jsonplaceholder.typicode.com/posts/1
```

Si la respuesta es exitosa, obtendremos `200` como salida.

## 10. Ejemplos Completos Usando cURL con una API Hypotética

Supongamos una API real donde se gestionan productos y clientes, similar al sistema que describimos en apartados anteriores, y veamos cómo utilizar `curl` con esa API. 

### 10.1. Obteniendo la Lista de Productos

```bash
curl https://api.miapp.com/productos
```

**Salida esperada**: Lista de productos en formato JSON:

```json
[
  {
    "id": 1,
    "nombre": "Camiseta",
    "precio": 15.99
  },
  {
    "id": 2,
    "nombre": "Pantalón",
    "precio": 29.99
  }
]
```

### 10.2. Obteniendo los Detalles de un Producto

```bash
curl https://api.miapp.com/productos/1
```

**Salida esperada**: Detalles del producto con ID 1:

```json
{
  "id": 1,
  "nombre": "Camiseta",
  "precio": 15.99
}
```

### 10.3. Creando un Nuevo Producto

```bash
curl -X POST \
     -H "Content-Type: application/json" \
     -d '{"nombre": "Zapatos", "precio": 45.99}' \
     https://api.miapp.com/productos
```

**Salida esperada**: El nuevo producto creado con su ID asignado:

```json
{
  "id": 3,
  "nombre": "Zapatos",
  "precio": 45.99
}
```

### 10.4. Actualizando Completamente un Producto

```bash
curl -X PUT \
     -H "Content-Type: application/json" \
     -d '{"id": 3, "nombre": "Zapatos deportivos", "precio": 49.99}' \
     https://api.miapp.com/productos/3
```

**Salida esperada**: El producto actualizado:

```json
{
  "id": 3,
  "nombre": "Zapatos deportivos",
  "precio": 49.99
}
```

### 10.5. Actualizando Parcialmente un Producto

```bash
curl -X PATCH \
     -H "Content-Type: application/json" \
     -d '{"precio": 39.99}' \
     https://api.miapp.com/productos/3
```

**Salida esperada**: El producto con el precio actualizado:

```json
{
  "id": 3,
  "nombre": "Zapatos deportivos",
  "precio": 39.99
}
```

### 10.6. Eliminando un Producto

```bash
curl -X DELETE https://api.miapp.com/productos/3
```

**Salida esperada**: Una respuesta indicando el éxito de la operación. Por ejemplo:

- Código de estado `204` sin contenido, o
- Un cuerpo con un mensaje de confirmación: 
```json
{
  "mensaje": "Producto eliminado correctamente"
}
```

## 11. Conclusión

- **`curl`** es una herramienta esencial para interactuar con APIs REST desde la línea de comandos.
- Puedes realizar todos los tipos de peticiones HTTP (GET, POST, PUT, PATCH, DELETE), enviar encabezados y datos en formato JSON, y leer y procesar las respuestas del servidor.
- Usando APIs públicas como JSONPlaceholder, puedes practicar y probar peticiones `curl` en diversos escenarios sin necesitar un entorno de servidor propio.
- Para APIs reales, probablemente necesitarás autenticarte mediante encabezados de autorización. `curl` facilita el envío de estos encabezados y cualquier otro campo necesario.
- Con la combinación adecuada de opciones (`-X`, `-H`, `-d`, `-i`, etc.), puedes simular la mayoría de las interacciones posibles con un API REST, lo que es extremadamente útil para pruebas y desarrollo.

Gracias a estos ejemplos y explicaciones, deberías sentirte más cómodo usando `curl` para realizar y probar peticiones REST, así como comprender mejor cómo formar las solicitudes HTTP y cómo interpretar las respuestas del servidor. Esto te permitirá verificar rápidamente el comportamiento de la API, depurar problemas y realizar integraciones con otras herramientas automatizadas.