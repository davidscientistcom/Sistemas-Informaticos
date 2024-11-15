## 1. Definición y Uso de Métodos HTTP 

En una API REST, los métodos HTTP representan acciones específicas que se realizarán sobre los recursos. Cada método tiene un propósito claro:

- **GET**: Obtener una representación de un recurso o una colección de recursos.
- **POST**: Crear un nuevo recurso.
- **PUT**: Actualizar un recurso existente.
- **PATCH**: Actualizar parcialmente un recurso existente (generalmente para cambiar unos pocos campos).
- **DELETE**: Eliminar un recurso existente.

### Ejemplo:
- `GET /productos`: Obtiene la lista completa de productos.
- `GET /productos/{id}`: Obtiene un producto específico por su ID.
- `POST /productos`: Crea un nuevo producto.
- `PUT /productos/{id}`: Actualiza completamente un producto existente.
- `PATCH /productos/{id}`: Actualiza parcialmente un producto existente.
- `DELETE /productos/{id}`: Elimina un producto específico por su ID.

Estas convenciones evitan la ambigüedad y permiten que los desarrolladores comprendan claramente la funcionalidad de cada endpoint.

## 2. Reglas para la Creación de Recursos y Rutas

**2.1. Usar nombres de recursos en plural**: 
Se recomienda usar sustantivos en plural para representar la colección de recursos en las URLs:
- Ejemplo: Para acceder a una lista de productos, utilizar `/productos`.

**2.2. Identificadores únicos de recursos**:
Los recursos individuales se acceden usando un identificador único en la ruta:
- Ejemplo: Para acceder a un producto específico por su ID, utilizar `/productos/{productoId}`.

**2.3. Subrecursos**:
Si un recurso tiene subrecursos directamente relacionados, estos pueden representarse en rutas anidadas:
- Ejemplo: Si un producto tiene varias reseñas, la ruta para acceder a las reseñas de un producto podría ser `/productos/{productoId}/reseñas`.

**2.4. Consistencia en el uso de métodos y rutas**:
Cada método HTTP debe corresponder consistentemente a una acción de la API:
- `GET` se emplea para leer, 
- `POST` para crear, 
- `PUT` y `PATCH` para actualizar y 
- `DELETE` para eliminar.

**2.5. Usar minúsculas y guiones medios (`-`) para separar palabras**:
- Se recomiendan URLs en minúsculas y palabras separadas por guiones, en caso necesario: 
  - Ejemplo: `/productos/destacados`.

**2.6. Evitar el uso de verbos en la ruta**:
Las rutas deben centrarse en los recursos, no en acciones. Los verbos se expresan a través de los métodos HTTP, no en la URL.
- Incorrecto: `/obtenerProductos`  
- Correcto: `GET /productos`

**2.7. Manejo de recursos complejos**:
Cuando se trabaja con recursos que pueden tener relaciones complejas (por ejemplo, productos y sus categorías), se deben exponer rutas que reflejen dichas relaciones.
- Ejemplo: Para acceder a las categorías de un producto, usar `/productos/{productoId}/categorias`.

## 3. Diseño de Rutas con Ejemplos Prácticos

A continuación, se presentan ejemplos de rutas para un sistema de gestión de productos y clientes, siguiendo las mejores prácticas REST:

### 3.1. Recursos para Productos

**Colección de productos**:
- `GET /productos`: Obtiene la lista de todos los productos.
- `POST /productos`: Crea un nuevo producto (el cuerpo de la petición incluirá los detalles del producto).

**Producto individual**:
- `GET /productos/{productoId}`: Obtiene los detalles de un producto específico.
- `PUT /productos/{productoId}`: Actualiza completamente un producto existente.
- `PATCH /productos/{productoId}`: Actualiza parcialmente un producto existente (por ejemplo, solo el precio o la descripción).
- `DELETE /productos/{productoId}`: Elimina un producto existente.

**Ejemplo de respuestas**:
- `GET /productos` podría devolver:
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

**Subrecursos de productos**:
- `GET /productos/{productoId}/reseñas`: Obtiene las reseñas para un producto específico.
- `POST /productos/{productoId}/reseñas`: Añade una nueva reseña a un producto.

**Ejemplo**:
- `GET /productos/1/reseñas` podría devolver:
  ```json
  [
    {
      "id": 101,
      "comentario": "Muy buena calidad",
      "estrellas": 5
    },
    {
      "id": 102,
      "comentario": "La entrega fue rápida",
      "estrellas": 4
    }
  ]
  ```

### 3.2. Recursos para Clientes

**Colección de clientes**:
- `GET /clientes`: Obtiene la lista de todos los clientes.
- `POST /clientes`: Crea un nuevo cliente.

**Cliente individual**:
- `GET /clientes/{clienteId}`: Obtiene la información de un cliente específico.
- `PUT /clientes/{clienteId}`: Actualiza completamente la información del cliente.
- `PATCH /clientes/{clienteId}`: Actualiza parcialmente la información del cliente (por ejemplo, la dirección de correo electrónico).
- `DELETE /clientes/{clienteId}`: Elimina un cliente.

**Ejemplo de respuestas**:
- `GET /clientes` podría devolver:
  ```json
  [
    {
      "id": 1,
      "nombre": "Juan Pérez",
      "email": "juan.perez@example.com"
    },
    {
      "id": 2,
      "nombre": "María López",
      "email": "maria.lopez@example.com"
    }
  ]
  ```

**Subrecursos de clientes**:
- `GET /clientes/{clienteId}/pedidos`: Obtiene todos los pedidos realizados por un cliente.
- `POST /clientes/{clienteId}/pedidos`: Crea un nuevo pedido para un cliente específico.

**Ejemplo**:
- `GET /clientes/1/pedidos` podría devolver:
  ```json
  [
    {
      "pedidoId": 201,
      "fecha": "2023-03-15",
      "total": 45.98
    },
    {
      "pedidoId": 202,
      "fecha": "2023-04-02",
      "total": 29.99
    }
  ]
  ```

## 4. Otras Reglas y Consideraciones

**4.1. Versionado de la API**:
Para las APIs públicas o que necesitan mantenerse estables, es aconsejable incluir la versión en la ruta:
- Ejemplo: `/v1/productos`  
De esta manera, las actualizaciones mayores que introduzcan cambios importantes en la API pueden lanzarse en nuevas rutas, como `/v2/productos`.

**4.2. Filtrado y Ordenación**:
En el caso de colecciones grandes, se deben permitir parámetros de consulta (`query params`) para filtrar, paginar u ordenar los resultados:
- Ejemplo: `GET /productos?categoria=ropa&orden=precio_asc` obtiene productos filtrados por la categoría "ropa" y los ordena ascendentemente por precio.
- Ejemplo: `GET /productos?pagina=2&tamaño=50` obtendría la segunda página con 50 resultados por página (paginación).

**4.3. Formato de respuesta y encabezados HTTP**:
El formato predeterminado suele ser JSON, pero se pueden soportar otros formatos (como XML). Esto se gestiona típicamente a través del encabezado `Accept` en la petición. 
- Ejemplo: `Accept: application/json`.

**4.4. Codes de estado HTTP**:
Para comunicarse correctamente con el cliente, la API debe devolver códigos de estado apropiados:
- `200 OK`: Éxito en la obtención de un recurso.
- `201 Created`: Éxito en la creación de un recurso.
- `204 No Content`: Éxito en la eliminación de un recurso, sin cuerpo de respuesta.
- `400 Bad Request`: Error en la solicitud (datos inválidos).
- `404 Not Found`: El recurso solicitado no existe.
- `500 Internal Server Error`: Error genérico en el servidor.

**4.5. Uso de HATEOAS (opcional avanzado)**:
`Hypermedia as the Engine of Application State` es un concepto avanzado de REST donde las respuestas contienen enlaces (URL) a recursos relacionados, permitiendo la navegación dinámica de la API:
- Ejemplo: Al obtener un producto, en la respuesta JSON puede haber un campo `_links` con enlaces a acciones relacionadas, como reseñas o actualización.
