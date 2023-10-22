## ¿Qué es REST?

REST es un estilo arquitectónico para la interacción entre los componentes de un sistema distribuido en una red informática. El acrónimo REST significa Transferencia de Estado Representacional (Representational State Transfer).

### REST se basa en los siguientes principios:

- Arquitectura cliente-servidor: El sistema está dividido en dos partes: un cliente que solicita recursos y un servidor que proporciona esos recursos.
- Recursos identificados de forma única: Cada recurso tiene una URL única que lo identifica.
- Operaciones a través de hipermedia: Las operaciones se realizan mediante métodos HTTP.
- Estado del recurso: El estado del recurso se representa en la respuesta HTTP.
- Cacheo: Los recursos se pueden almacenar en caché para mejorar el rendimiento.

### REST utiliza los métodos HTTP para realizar operaciones sobre los recursos. Los métodos HTTP más comunes son:

- GET: Obtiene la representación de un recurso.
- POST: Crea un nuevo recurso.
- PUT: Actualiza un recurso existente.
- DELETE: Elimina un recurso existente.

### Formatos de datos

REST utiliza formatos de datos estándar para representar los recursos. Los formatos de datos más comunes son:

- JSON: JavaScript Object Notation.
- XML: Extensible Markup Language.

### Ejemplo de REST

Un ejemplo de REST es el servicio web que proporciona información sobre el clima. Este servicio web tiene un recurso para cada ciudad. El estado del recurso es la información sobre el clima de la ciudad. El cliente puede realizar las siguientes operaciones sobre los recursos:

- GET: Obtener la información sobre el clima de una ciudad.
- POST: Crear una nueva ciudad.
- PUT: Actualizar la información sobre el clima de una ciudad.
- DELETE: Eliminar una ciudad.