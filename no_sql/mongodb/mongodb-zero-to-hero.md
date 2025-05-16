# MongoDB: De Cero a Héroe - Guía Completa

## Índice
1. [Introducción a MongoDB](#introducción-a-mongodb)
2. [Instalación de MongoDB](#instalación-de-mongodb)
3. [MongoDB Compass: Interfaz Gráfica](#mongodb-compass-interfaz-gráfica)
4. [Conceptos Fundamentales de MongoDB](#conceptos-fundamentales-de-mongodb)
5. [Operaciones DDL (Data Definition Language)](#operaciones-ddl-data-definition-language)
6. [Operaciones DML (Data Manipulation Language)](#operaciones-dml-data-manipulation-language)
7. [Consultas Avanzadas](#consultas-avanzadas)
8. [Indexación en MongoDB](#indexación-en-mongodb)
9. [Framework de Agregación](#framework-de-agregación)
10. [MongoDB Atlas: La Nube de MongoDB](#mongodb-atlas-la-nube-de-mongodb)
11. [Seguridad en MongoDB](#seguridad-en-mongodb)
12. [Replicación y Sharding](#replicación-y-sharding)
13. [Optimización y Performance](#optimización-y-performance)
14. [Patrones de Diseño con MongoDB](#patrones-de-diseño-con-mongodb)
15. [MongoDB con Aplicaciones (Drivers)](#mongodb-con-aplicaciones-drivers)
16. [Recursos y Siguiente Nivel](#recursos-y-siguiente-nivel)

## Introducción a MongoDB

### ¿Qué es MongoDB?

MongoDB es una base de datos NoSQL orientada a documentos, diseñada para ofrecer escalabilidad, rendimiento y alta disponibilidad. A diferencia de las bases de datos relacionales tradicionales, MongoDB almacena los datos en documentos tipo JSON con esquemas dinámicos, lo que significa que no requiere una estructura predefinida.

### Características principales

- **Orientado a documentos**: Almacena datos en documentos tipo JSON llamados BSON (Binary JSON)
- **Esquema flexible**: No requiere definir la estructura de los documentos de antemano
- **Alta escalabilidad**: Soporta sharding (fragmentación) para distribuir datos entre múltiples servidores
- **Alto rendimiento**: Incluye índices, consultas geoespaciales y procesamiento de texto completo
- **Alta disponibilidad**: Proporciona replicación con conjuntos de réplicas para garantizar la disponibilidad

### Casos de uso de MongoDB

- Aplicaciones con datos de alta variabilidad
- Aplicaciones de alta velocidad de datos (Big Data)
- Aplicaciones de comercio electrónico
- Gestión de contenido y datos de usuarios
- Aplicaciones móviles
- Analítica en tiempo real
- Internet de las cosas (IoT)

### Ventajas vs Bases de Datos Relacionales

- Esquema flexible que permite cambios rápidos
- Escalabilidad horizontal sin necesidad de complejos joins
- Mejor rendimiento para operaciones de lectura/escritura de gran volumen
- Representación nativa de datos en formato que utilizan muchas aplicaciones modernas

## Instalación de MongoDB

### Instalación en Windows

1. **Descarga el instalador**:
   - Visita [MongoDB Download Center](https://www.mongodb.com/try/download/community)
   - Selecciona la versión "Community Server"
   - Elige "Windows" como plataforma
   - Selecciona "MSI" como tipo de paquete
   - Haz clic en "Download"

2. **Ejecuta el instalador**:
   - Ejecuta el archivo MSI descargado
   - Sigue las instrucciones del asistente de instalación
   - Selecciona "Complete" como tipo de instalación
   - Puedes activar "Install MongoDB as a Service" para que MongoDB se inicie automáticamente

3. **Verifica la instalación**:
   - Abre el Command Prompt o PowerShell
   - Navega a la carpeta bin de MongoDB (por defecto: `C:\Program Files\MongoDB\Server\{version}\bin`)
   - Ejecuta `mongo` para abrir la consola de MongoDB

### Instalación en macOS

1. **Usando Homebrew**:
   ```bash
   # Instalar Homebrew si no lo tienes
   /bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
   
   # Instalar MongoDB
   brew tap mongodb/brew
   brew install mongodb-community
   
   # Iniciar MongoDB
   brew services start mongodb-community
   ```

2. **Verificar la instalación**:
   ```bash
   # Conectar al servidor de MongoDB
   mongosh
   ```

### Instalación en Linux (Ubuntu)

1. **Importar la clave pública de MongoDB**:
   ```bash
   wget -qO - https://www.mongodb.org/static/pgp/server-6.0.asc | sudo apt-key add -
   ```

2. **Crear archivo de lista para MongoDB**:
   ```bash
   echo "deb [ arch=amd64,arm64 ] https://repo.mongodb.org/apt/ubuntu focal/mongodb-org/6.0 multiverse" | sudo tee /etc/apt/sources.list.d/mongodb-org-6.0.list
   ```

3. **Actualizar índice de paquetes**:
   ```bash
   sudo apt-get update
   ```

4. **Instalar MongoDB**:
   ```bash
   sudo apt-get install -y mongodb-org
   ```

5. **Iniciar MongoDB**:
   ```bash
   sudo systemctl start mongod
   ```

6. **Verificar el estado**:
   ```bash
   sudo systemctl status mongod
   ```

7. **Configurar el inicio automático**:
   ```bash
   sudo systemctl enable mongod
   ```

8. **Conectar a MongoDB**:
   ```bash
   mongosh
   ```

### Instalación con Docker

1. **Descargar la imagen de Docker**:
   ```bash
   docker pull mongo
   ```

2. **Ejecutar el contenedor de MongoDB**:
   ```bash
   docker run --name mongodb -d -p 27017:27017 mongo
   ```

3. **Conectar al servidor de MongoDB en el contenedor**:
   ```bash
   docker exec -it mongodb mongosh
   ```

## MongoDB Compass: Interfaz Gráfica

MongoDB Compass es la interfaz gráfica oficial para MongoDB que permite explorar, manipular y visualizar datos fácilmente.

### Instalación de MongoDB Compass

1. **Descargar Compass**:
   - Visita [MongoDB Compass Download](https://www.mongodb.com/try/download/compass)
   - Selecciona la versión "Stable" o "Beta"
   - Elige tu sistema operativo
   - Descarga el instalador

2. **Instalar Compass**:
   - Ejecuta el instalador descargado
   - Sigue las instrucciones del asistente de instalación

### Conexión a MongoDB con Compass

1. **Iniciar Compass**:
   - Abre la aplicación MongoDB Compass

2. **Crear una nueva conexión**:
   - Para una instalación local usa: `mongodb://localhost:27017`
   - Para conectar a un servidor remoto: `mongodb://usuario:contraseña@host:puerto/base-de-datos?authSource=admin`

3. **Opciones de conexión avanzadas**:
   - SSL/TLS
   - Autenticación
   - Opciones de conexión
   - Favoritos y conexiones recientes

### Funcionalidades principales de Compass

- **Explorador de bases de datos**: Navegar por bases de datos y colecciones
- **Visor de documentos**: Ver y editar documentos individualmente
- **Editor de consultas**: Crear, guardar y ejecutar consultas con sintaxis MongoDB
- **Análisis de esquema**: Visualizar la estructura de los documentos en una colección
- **Panel de rendimiento**: Monitorear el rendimiento de la base de datos
- **Importar/exportar datos**: Cargar datos desde archivos CSV, JSON o BSON
- **Agregaciones**: Crear y probar pipelines de agregación con un constructor visual
- **Índices**: Crear, gestionar y visualizar el impacto de los índices

### Ejemplo de uso de Compass

1. **Crear una nueva base de datos**:
   - Haz clic en "Create Database"
   - Ingresa el nombre de la base de datos y la colección inicial

2. **Insertar documentos**:
   - Selecciona la colección
   - Haz clic en "Add Data" -> "Insert Document"
   - Ingresa los datos en formato JSON
   - Haz clic en "Insert"

3. **Realizar consultas**:
   - Selecciona la colección
   - En la barra de filtros, ingresa tu consulta (ej. `{edad: {$gt: 30}}`)
   - Haz clic en "Find"

4. **Crear índices**:
   - Selecciona la colección
   - Ve a la pestaña "Indexes"
   - Haz clic en "Create Index"
   - Configura los campos y opciones
   - Haz clic en "Create"

## Conceptos Fundamentales de MongoDB

### Estructura de datos en MongoDB

La jerarquía de datos en MongoDB sigue este orden:

1. **Servidor MongoDB**: La instancia del servidor
2. **Bases de datos**: Contenedores para colecciones
3. **Colecciones**: Grupos de documentos (equivalentes a tablas en RDBMS)
4. **Documentos**: Registros individuales en formato BSON (equivalentes a filas)
5. **Campos**: Pares clave-valor dentro de documentos (equivalentes a columnas)

### Documentos BSON

- BSON (Binary JSON) es el formato de almacenamiento de MongoDB
- Extiende JSON con tipos de datos adicionales
- Los documentos tienen una limitación de tamaño de 16MB
- Cada documento tiene un identificador único `_id`

Ejemplo de documento BSON:
```json
{
  "_id": ObjectId("60a5e8380c314f9f644a0416"),
  "nombre": "Juan",
  "apellido": "Pérez",
  "edad": 30,
  "email": "juan.perez@ejemplo.com",
  "direccion": {
    "calle": "Calle Principal",
    "numero": 123,
    "ciudad": "Madrid",
    "codigo_postal": "28001"
  },
  "telefonos": ["+34600123456", "+34910234567"],
  "activo": true,
  "fecha_registro": ISODate("2023-03-15T14:30:00Z")
}
```

### Tipos de datos en MongoDB

- **String**: Cadenas de texto UTF-8
- **Integer**: Números enteros (32 o 64 bits)
- **Double**: Números de punto flotante
- **Boolean**: Valores `true` o `false`
- **Date**: Fecha y hora (milisegundos desde la época Unix)
- **Object**: Documentos anidados/embebidos
- **Array**: Listas de valores o documentos
- **ObjectId**: Identificadores únicos generados automáticamente
- **Null**: Valor nulo o ausente
- **Binary data**: Datos binarios
- **Regular expression**: Expresiones regulares
- **JavaScript code**: Código JavaScript
- **Decimal128**: Números decimales de alta precisión
- **Timestamp**: Marcas de tiempo internas
- **MinKey/MaxKey**: Valores mínimos y máximos comparables

### Modelo de datos

MongoDB permite dos enfoques principales para modelar relaciones:

1. **Documentos embebidos (embedding)**: Almacenar datos relacionados dentro del mismo documento
   - Ventajas: Mayor rendimiento para lecturas, consultas en una sola operación
   - Desventajas: Posible duplicación de datos, limitación de tamaño (16MB)

2. **Referencias (referencing)**: Almacenar referencias entre documentos (similar a claves externas)
   - Ventajas: Sin duplicación, adecuado para relaciones muchos-a-muchos
   - Desventajas: Requiere múltiples consultas (operaciones JOIN manuales)

Ejemplo de documento embebido:
```json
{
  "_id": ObjectId("60a5e8380c314f9f644a0416"),
  "nombre": "Juan Pérez",
  "direccion": {
    "calle": "Calle Principal",
    "numero": 123,
    "ciudad": "Madrid"
  }
}
```

Ejemplo de documentos con referencias:
```json
// Colección "clientes"
{
  "_id": ObjectId("60a5e8380c314f9f644a0416"),
  "nombre": "Juan Pérez"
}

// Colección "pedidos"
{
  "_id": ObjectId("60a5e8380c314f9f644a0417"),
  "cliente_id": ObjectId("60a5e8380c314f9f644a0416"),
  "producto": "Laptop",
  "precio": 999.99
}
```

## Operaciones DDL (Data Definition Language)

En MongoDB, las operaciones DDL se refieren a las operaciones que manipulan la estructura de la base de datos, como crear, modificar o eliminar bases de datos y colecciones.

### Bases de Datos

#### Crear una base de datos
```javascript
// Configuración del motor de almacenamiento WiredTiger (mongod.conf)
storage:
  wiredTiger:
    engineConfig:
      cacheSizeGB: 2  // Tamaño de la caché (ajustar según RAM disponible)
      journalCompressor: snappy  // Algoritmo de compresión (snappy, zlib, none)
    collectionConfig:
      blockCompressor: snappy  // Compresión para colecciones
    indexConfig:
      prefixCompression: true  // Compresión de prefijos para índices
```

## Patrones de Diseño con MongoDB

MongoDB permite diferentes enfoques de modelado de datos que aprovechan su formato flexible de documentos. A continuación se presentan varios patrones comunes.

### Patrón de Documentos Embebidos (Embedding)

Ideal para relaciones uno-a-pocos donde los datos relacionados se consultan juntos frecuentemente.

```javascript
// Documento cliente con direcciones embebidas
db.clientes.insertOne({
  _id: ObjectId("60a5e8380c314f9f644a0416"),
  nombre: "Juan Pérez",
  email: "juan@ejemplo.com",
  direcciones: [
    {
      tipo: "casa",
      calle: "Av. Principal 123",
      ciudad: "Madrid",
      codigo_postal: "28001"
    },
    {
      tipo: "trabajo",
      calle: "Calle Comercial 45",
      ciudad: "Madrid",
      codigo_postal: "28045"
    }
  ]
})

// Consulta simple
db.clientes.find({ "direcciones.ciudad": "Madrid" })
```

**Ventajas**:
- Recuperación de datos relacionados en una sola consulta
- Mejor rendimiento de lectura
- Actualizaciones atómicas

**Desventajas**:
- Documentos potencialmente grandes
- Posible duplicación de datos
- Límite de tamaño del documento (16MB)

### Patrón de Referencias (Referencing)

Ideal para relaciones uno-a-muchos o muchos-a-muchos donde los datos relacionados se consultan independientemente.

```javascript
// Colección de autores
db.autores.insertOne({
  _id: ObjectId("60a5e8380c314f9f644a0417"),
  nombre: "Gabriel García Márquez",
  nacionalidad: "Colombiana"
})

// Colección de libros con referencia al autor
db.libros.insertOne({
  _id: ObjectId("60a5e8380c314f9f644a0418"),
  titulo: "Cien años de soledad",
  autor_id: ObjectId("60a5e8380c314f9f644a0417"),
  genero: "Realismo mágico",
  año: 1967
})

// Consulta con lookup (similar a JOIN)
db.libros.aggregate([
  {
    $match: { titulo: "Cien años de soledad" }
  },
  {
    $lookup: {
      from: "autores",
      localField: "autor_id",
      foreignField: "_id",
      as: "autor"
    }
  }
])
```

**Ventajas**:
- Documentos más pequeños
- Sin duplicación de datos
- Actualización de datos normalizados más simple

**Desventajas**:
- Múltiples consultas o operaciones de agregación
- Rendimiento de lectura potencialmente menor

### Patrón de Documentos Extendidos

Combina embedding y referencing para optimizar casos de uso específicos.

```javascript
// Producto con detalles básicos embebidos y referencia para información extendida
db.productos.insertOne({
  _id: ObjectId("60a5e8380c314f9f644a0419"),
  nombre: "Smartphone XYZ",
  precio: 699.99,
  categoria: "Electrónica",
  valoraciones: {
    promedio: 4.7,
    total: 328
  },
  // Referencia a detalles extendidos
  detalles_id: ObjectId("60a5e8380c314f9f644a0420")
})

// Detalles extendidos del producto
db.detalles_producto.insertOne({
  _id: ObjectId("60a5e8380c314f9f644a0420"),
  producto_id: ObjectId("60a5e8380c314f9f644a0419"),
  especificaciones: {
    pantalla: "6.5 pulgadas AMOLED",
    procesador: "Octa-core 2.4GHz",
    memoria: "8GB RAM, 128GB almacenamiento",
    bateria: "4500mAh",
    camara: "48MP + 12MP + 5MP"
  },
  descripcion_larga: "Texto muy largo con la descripción completa del producto...",
  historial_precios: [
    { fecha: ISODate("2023-01-01"), precio: 799.99 },
    { fecha: ISODate("2023-03-15"), precio: 749.99 },
    { fecha: ISODate("2023-06-01"), precio: 699.99 }
  ]
})
```

### Patrón de Esquema de Facetas (Schema Versioning)

Útil cuando la estructura de los documentos evoluciona con el tiempo.

```javascript
// Versión 1 del esquema
db.usuarios.insertOne({
  _id: ObjectId("60a5e8380c314f9f644a0421"),
  schema_version: 1,
  nombre: "Ana García",
  email: "ana@ejemplo.com"
})

// Versión 2 del esquema con campos adicionales
db.usuarios.insertOne({
  _id: ObjectId("60a5e8380c314f9f644a0422"),
  schema_version: 2,
  nombre: "Carlos López",
  email: "carlos@ejemplo.com",
  telefono: "+34600123456",
  preferencias: {
    tema: "oscuro",
    notificaciones: true
  }
})

// Consulta con manejo de versiones
db.usuarios.find({
  $or: [
    { schema_version: 1, nombre: /^A/ },
    { schema_version: 2, nombre: /^A/, "preferencias.tema": "oscuro" }
  ]
})
```

### Patrón de Árboles (Trees)

Para representar estructuras jerárquicas como categorías, árboles de comentarios o estructuras organizativas.

#### Modelo de Array de Ancestros

```javascript
// Categorías con array de ancestros
db.categorias.insertMany([
  {
    _id: ObjectId("60a5e8380c314f9f644a0423"),
    nombre: "Electrónica",
    path: []  // Categoría raíz
  },
  {
    _id: ObjectId("60a5e8380c314f9f644a0424"),
    nombre: "Teléfonos",
    path: [ObjectId("60a5e8380c314f9f644a0423")]  // Hijo de Electrónica
  },
  {
    _id: ObjectId("60a5e8380c314f9f644a0425"),
    nombre: "Smartphones",
    path: [
      ObjectId("60a5e8380c314f9f644a0423"),
      ObjectId("60a5e8380c314f9f644a0424")
    ]  // Nieto de Electrónica, hijo de Teléfonos
  }
])

// Consultar todos los descendientes de Electrónica
db.categorias.find({
  path: ObjectId("60a5e8380c314f9f644a0423")
})

// Consultar toda la ruta de un nodo
db.categorias.find({
  $or: [
    { _id: ObjectId("60a5e8380c314f9f644a0425") },
    { _id: { $in: db.categorias.findOne({ _id: ObjectId("60a5e8380c314f9f644a0425") }).path } }
  ]
})
```

#### Modelo de Referencias Padre/Hijo

```javascript
// Comentarios en un blog
db.comentarios.insertMany([
  {
    _id: ObjectId("60a5e8380c314f9f644a0426"),
    texto: "¡Gran artículo!",
    articulo_id: ObjectId("60a5e8380c314f9f644a0430"),
    autor: "Usuario1",
    fecha: ISODate("2023-06-10T10:00:00Z"),
    padre_id: null  // Comentario raíz
  },
  {
    _id: ObjectId("60a5e8380c314f9f644a0427"),
    texto: "Estoy de acuerdo contigo",
    articulo_id: ObjectId("60a5e8380c314f9f644a0430"),
    autor: "Usuario2",
    fecha: ISODate("2023-06-10T10:30:00Z"),
    padre_id: ObjectId("60a5e8380c314f9f644a0426")  // Respuesta al primer comentario
  },
  {
    _id: ObjectId("60a5e8380c314f9f644a0428"),
    texto: "Gracias por tu comentario",
    articulo_id: ObjectId("60a5e8380c314f9f644a0430"),
    autor: "Usuario1",
    fecha: ISODate("2023-06-10T11:00:00Z"),
    padre_id: ObjectId("60a5e8380c314f9f644a0427")  // Respuesta al segundo comentario
  }
])

// Función recursiva para obtener comentarios anidados
function getCommentTree(rootCommentId) {
  let comment = db.comentarios.findOne({ _id: rootCommentId });
  comment.respuestas = db.comentarios.find({ padre_id: rootCommentId }).map(getCommentTree);
  return comment;
}

// Obtener todos los comentarios raíz y sus respuestas
db.comentarios.find({ padre_id: null, articulo_id: ObjectId("60a5e8380c314f9f644a0430") }).map(getCommentTree);
```

### Patrón de Transacciones de Dos Fases (Two-Phase Commits)

Para operaciones que requieren transacciones entre múltiples colecciones (antes de MongoDB 4.0 que introdujo transacciones multi-documento).

```javascript
// Transferencia bancaria
// 1. Iniciar la transacción
db.transacciones.insertOne({
  _id: ObjectId("60a5e8380c314f9f644a0429"),
  cuenta_origen: "123456",
  cuenta_destino: "789012",
  cantidad: 500,
  estado: "pendiente",
  fecha_inicio: new Date()
})

// 2. Actualizar la cuenta origen
db.cuentas.updateOne(
  { numero: "123456", saldo: { $gte: 500 } },
  { 
    $inc: { saldo: -500 },
    $push: { 
      movimientos: {
        tipo: "transferencia_saliente",
        cantidad: 500,
        transaccion_id: ObjectId("60a5e8380c314f9f644a0429"),
        fecha: new Date()
      }
    }
  }
)

// 3. Actualizar la cuenta destino
db.cuentas.updateOne(
  { numero: "789012" },
  { 
    $inc: { saldo: 500 },
    $push: { 
      movimientos: {
        tipo: "transferencia_entrante",
        cantidad: 500,
        transaccion_id: ObjectId("60a5e8380c314f9f644a0429"),
        fecha: new Date()
      }
    }
  }
)

// 4. Finalizar la transacción
db.transacciones.updateOne(
  { _id: ObjectId("60a5e8380c314f9f644a0429") },
  {
    $set: {
      estado: "completada",
      fecha_fin: new Date()
    }
  }
)
```

### Patrón de Subcollecciones

Para organizar datos relacionados en colecciones separadas con nombres que indican relación.

```javascript
// Colección principal
db.productos.insertOne({
  _id: ObjectId("60a5e8380c314f9f644a0430"),
  nombre: "Laptop Pro X",
  precio: 1299.99
})

// Subcollecciones con prefijo
db.productos_specs.insertOne({
  producto_id: ObjectId("60a5e8380c314f9f644a0430"),
  cpu: "Intel i7",
  ram: "16GB",
  almacenamiento: "512GB SSD"
})

db.productos_reviews.insertMany([
  {
    producto_id: ObjectId("60a5e8380c314f9f644a0430"),
    usuario: "Cliente1",
    puntuacion: 5,
    comentario: "Excelente laptop"
  },
  {
    producto_id: ObjectId("60a5e8380c314f9f644a0430"),
    usuario: "Cliente2",
    puntuacion: 4,
    comentario: "Muy buena, pero la batería podría durar más"
  }
])
```

## MongoDB con Aplicaciones (Drivers)

MongoDB proporciona drivers oficiales para múltiples lenguajes de programación. A continuación, se presentan ejemplos de código para algunos de los lenguajes más populares.

### Node.js

#### Instalación
```bash
npm install mongodb
```

#### Conexión y operaciones básicas
```javascript
const { MongoClient } = require('mongodb');

// URI de conexión
const uri = "mongodb://localhost:27017";
const client = new MongoClient(uri);

async function main() {
  try {
    // Conectar al servidor
    await client.connect();
    console.log("Conectado a MongoDB");
    
    // Seleccionar base de datos y colección
    const database = client.db("miBaseDeDatos");
    const usuarios = database.collection("usuarios");
    
    // Insertar un documento
    const insertResult = await usuarios.insertOne({
      nombre: "María",
      email: "maria@ejemplo.com",
      edad: 28
    });
    console.log(`Documento insertado con ID: ${insertResult.insertedId}`);
    
    // Buscar documentos
    const cursor = usuarios.find({ edad: { $gt: 25 } });
    const results = await cursor.toArray();
    console.log("Documentos encontrados:");
    console.log(results);
    
    // Actualizar un documento
    const updateResult = await usuarios.updateOne(
      { nombre: "María" },
      { $set: { edad: 29 } }
    );
    console.log(`${updateResult.modifiedCount} documento(s) actualizado(s)`);
    
    // Eliminar un documento
    const deleteResult = await usuarios.deleteOne({ nombre: "María" });
    console.log(`${deleteResult.deletedCount} documento(s) eliminado(s)`);
    
  } finally {
    // Cerrar la conexión
    await client.close();
  }
}

main().catch(console.error);
```

#### Ejemplo con Express.js
```javascript
const express = require('express');
const { MongoClient, ObjectId } = require('mongodb');

const app = express();
app.use(express.json());

const uri = "mongodb://localhost:27017";
const client = new MongoClient(uri);

// Conexión a la base de datos
async function connectDB() {
  try {
    await client.connect();
    return client.db("miBaseDeDatos");
  } catch (error) {
    console.error("Error conectando a MongoDB:", error);
    process.exit(1);
  }
}

// Rutas
app.get('/usuarios', async (req, res) => {
  try {
    const db = await connectDB();
    const usuarios = await db.collection("usuarios").find().toArray();
    res.json(usuarios);
    }
}

// Rutas (routes/api.php)
Route::apiResource('usuarios', UsuarioController::class);
Route::get('usuarios/edad/{edad}', [UsuarioController::class, 'findByAge']);
```

## Recursos y Siguiente Nivel

Para continuar aprendiendo y profundizando tus conocimientos sobre MongoDB, aquí te presentamos varios recursos y consejos para llevarte al siguiente nivel.

### Recursos oficiales

- **[Documentación oficial de MongoDB](https://docs.mongodb.com/)**: La documentación completa de MongoDB, incluyendo guías, referencias y tutoriales.
- **[MongoDB University](https://university.mongodb.com/)**: Cursos gratuitos de MongoDB, desde nivel principiante hasta avanzado, con certificaciones oficiales.
- **[MongoDB Developer Center](https://www.mongodb.com/developer/)**: Artículos, tutoriales y recursos para desarrolladores.
- **[GitHub de MongoDB](https://github.com/mongodb/mongo)**: Código fuente y ejemplos oficiales.

### Libros recomendados

- "MongoDB: The Definitive Guide" - Kristina Chodorow
- "MongoDB in Action" - Kyle Banker
- "MongoDB Applied Design Patterns" - Rick Copeland
- "Scaling MongoDB" - Kristina Chodorow
- "MongoDB Performance Tuning" - Guy Harrison

### Herramientas y utilidades

- **[MongoDB Compass](https://www.mongodb.com/products/compass)**: Interfaz gráfica oficial para MongoDB.
- **[Studio 3T](https://studio3t.com/)**: IDE profesional para MongoDB con múltiples características avanzadas.
- **[Mongosh](https://docs.mongodb.com/mongodb-shell/)**: La nueva shell de MongoDB con mejoras en la experiencia de usuario.
- **[MongoDB Charts](https://www.mongodb.com/products/charts)**: Herramienta de visualización de datos integrada con MongoDB.
- **[Mongoose](https://mongoosejs.com/)**: ODM (Object Document Mapper) para MongoDB y Node.js.
- **[Spring Data MongoDB](https://spring.io/projects/spring-data-mongodb)**: Soporte de MongoDB para aplicaciones Spring.

### Comunidad y foros

- **[MongoDB Community Forums](https://www.mongodb.com/community/forums/)**: Foros oficiales de la comunidad MongoDB.
- **[Stack Overflow - MongoDB](https://stackoverflow.com/questions/tagged/mongodb)**: Preguntas y respuestas etiquetadas con MongoDB.
- **[Reddit - r/mongodb](https://www.reddit.com/r/mongodb/)**: Subreddit dedicado a MongoDB.
- **[MongoDB Meetups](https://www.mongodb.com/community/meetups)**: Grupos de usuarios locales en todo el mundo.

### Certificaciones de MongoDB

MongoDB ofrece certificaciones oficiales para validar tus conocimientos:

1. **MongoDB Certified Developer Associate**: Para desarrolladores que utilizan MongoDB en sus aplicaciones.
2. **MongoDB Certified DBA Associate**: Para administradores de bases de datos MongoDB.
3. **MongoDB Certified Developer Professional**: Nivel avanzado para desarrolladores.
4. **MongoDB Certified DBA Professional**: Nivel avanzado para administradores.

### Próximos pasos en tu aprendizaje

1. **Dominar el modelado de datos**: Profundiza en los patrones de diseño y aprende a modelar datos específicos para tu dominio.

2. **Escalabilidad y rendimiento**:
   - Aprende sobre sharding y replicación avanzada
   - Técnicas de optimización para grandes volúmenes de datos
   - Diagnóstico y resolución de problemas de rendimiento

3. **MongoDB en producción**:
   - Implementación de estrategias de backup y recuperación
   - Monitorización y alertas
   - Optimización de servidores y sistemas operativos para MongoDB

4. **Integración con otras tecnologías**:
   - Big Data y análisis (Hadoop, Spark)
   - Streaming de datos (Kafka, RabbitMQ)
   - Machine Learning con MongoDB

5. **Desarrollo de habilidades específicas**:
   - Desarrollo de agregaciones complejas
   - Creación de pipelines de datos
   - Implementación de búsqueda de texto completo avanzada

### Proyectos prácticos para mejorar

1. **Sistema de gestión de contenido**: Desarrolla un CMS usando MongoDB para almacenar artículos, usuarios y comentarios.

2. **Aplicación de comercio electrónico**: Implementa un sistema de carrito de compras, productos y pedidos.

3. **Dashboard de análisis**: Crea una aplicación que muestre métricas y estadísticas a partir de datos almacenados en MongoDB.

4. **API RESTful**: Desarrolla una API completa con autenticación, autorización y operaciones CRUD.

5. **Sistema de geolocalización**: Utiliza las capacidades geoespaciales de MongoDB para crear una aplicación basada en ubicación.

6. **Motor de búsqueda personalizado**: Implementa búsqueda de texto completo con Atlas Search o índices de texto.

7. **Sistema de gestión de eventos en tiempo real**: Utiliza change streams para crear notificaciones en tiempo real.

### Conclusión

MongoDB es una base de datos potente y flexible que continúa evolucionando con nuevas características y mejoras. Desde sus inicios como una base de datos NoSQL orientada a documentos, ha crecido hasta convertirse en una plataforma de datos completa con capacidades para una amplia gama de casos de uso.

Este documento ha cubierto los conceptos fundamentales, instalación, operaciones CRUD, consultas avanzadas, indexación, agregación, seguridad, escalabilidad y patrones de diseño. Con estos conocimientos, ya tienes una base sólida para desarrollar aplicaciones eficientes y escalables con MongoDB.

Recuerda que el aprendizaje es un proceso continuo, especialmente en tecnología. Mantente actualizado con las nuevas versiones y características, participa en la comunidad y, lo más importante, practica regularmente para consolidar tus habilidades.

¡Buena suerte en tu viaje de MongoDB de Cero a Héroe!
  } catch (error) {
    res.status(500).json({ error: error.message });
  }
});

app.get('/usuarios/:id', async (req, res) => {
  try {
    const db = await connectDB();
    const usuario = await db.collection("usuarios").findOne({ 
      _id: new ObjectId(req.params.id) 
    });
    if (!usuario) {
      return res.status(404).json({ error: "Usuario no encontrado" });
    }
    res.json(usuario);
  } catch (error) {
    res.status(500).json({ error: error.message });
  }
});

app.post('/usuarios', async (req, res) => {
  try {
    const db = await connectDB();
    const result = await db.collection("usuarios").insertOne(req.body);
    res.status(201).json({ 
      id: result.insertedId,
      ...req.body 
    });
  } catch (error) {
    res.status(500).json({ error: error.message });
  }
});

const PORT = process.env.PORT || 3000;
app.listen(PORT, () => {
  console.log(`Servidor ejecutándose en el puerto ${PORT}`);
});
```

### Python

#### Instalación
```bash
pip install pymongo
```

#### Conexión y operaciones básicas
```python
from pymongo import MongoClient
from bson.objectid import ObjectId

# Conectar al servidor MongoDB
client = MongoClient('mongodb://localhost:27017/')

# Seleccionar base de datos y colección
db = client['miBaseDeDatos']
usuarios = db['usuarios']

# Insertar un documento
usuario_nuevo = {
    'nombre': 'Pablo',
    'email': 'pablo@ejemplo.com',
    'edad': 32
}
resultado_insercion = usuarios.insert_one(usuario_nuevo)
print(f"ID del documento insertado: {resultado_insercion.inserted_id}")

# Buscar documentos
for usuario in usuarios.find({'edad': {'$gt': 30}}):
    print(usuario)

# Buscar un documento específico
usuario = usuarios.find_one({'_id': resultado_insercion.inserted_id})
print(f"Usuario encontrado: {usuario}")

# Actualizar un documento
resultado_actualizacion = usuarios.update_one(
    {'nombre': 'Pablo'},
    {'$set': {'edad': 33}}
)
print(f"Documentos actualizados: {resultado_actualizacion.modified_count}")

# Eliminar un documento
resultado_eliminacion = usuarios.delete_one({'nombre': 'Pablo'})
print(f"Documentos eliminados: {resultado_eliminacion.deleted_count}")

# Cerrar la conexión
client.close()
```

#### Ejemplo con Flask
```python
from flask import Flask, request, jsonify
from pymongo import MongoClient
from bson.objectid import ObjectId
from bson.json_util import dumps
import json

app = Flask(__name__)

# Conexión a MongoDB
client = MongoClient('mongodb://localhost:27017/')
db = client['miBaseDeDatos']
usuarios = db['usuarios']

# Función para convertir ObjectId a string en las respuestas JSON
def parse_json(data):
    return json.loads(dumps(data))

@app.route('/usuarios', methods=['GET'])
def get_usuarios():
    try:
        result = list(usuarios.find())
        return jsonify(parse_json(result))
    except Exception as e:
        return jsonify({"error": str(e)}), 500

@app.route('/usuarios/<id>', methods=['GET'])
def get_usuario(id):
    try:
        usuario = usuarios.find_one({"_id": ObjectId(id)})
        if usuario:
            return jsonify(parse_json(usuario))
        return jsonify({"error": "Usuario no encontrado"}), 404
    except Exception as e:
        return jsonify({"error": str(e)}), 500

@app.route('/usuarios', methods=['POST'])
def add_usuario():
    try:
        usuario = request.json
        resultado = usuarios.insert_one(usuario)
        usuario['_id'] = str(resultado.inserted_id)
        return jsonify(usuario), 201
    except Exception as e:
        return jsonify({"error": str(e)}), 500

@app.route('/usuarios/<id>', methods=['PUT'])
def update_usuario(id):
    try:
        usuario = request.json
        resultado = usuarios.update_one(
            {"_id": ObjectId(id)},
            {"$set": usuario}
        )
        if resultado.modified_count > 0:
            return jsonify({"mensaje": "Usuario actualizado correctamente"})
        return jsonify({"error": "Usuario no encontrado o sin cambios"}), 404
    except Exception as e:
        return jsonify({"error": str(e)}), 500

@app.route('/usuarios/<id>', methods=['DELETE'])
def delete_usuario(id):
    try:
        resultado = usuarios.delete_one({"_id": ObjectId(id)})
        if resultado.deleted_count > 0:
            return jsonify({"mensaje": "Usuario eliminado correctamente"})
        return jsonify({"error": "Usuario no encontrado"}), 404
    except Exception as e:
        return jsonify({"error": str(e)}), 500

if __name__ == '__main__':
    app.run(debug=True, port=3000)
```

### Java

#### Dependencias (Maven)
```xml
<dependency>
    <groupId>org.mongodb</groupId>
    <artifactId>mongodb-driver-sync</artifactId>
    <version>4.9.1</version>
</dependency>
```

#### Conexión y operaciones básicas
```java
import com.mongodb.client.MongoClient;
import com.mongodb.client.MongoClients;
import com.mongodb.client.MongoCollection;
import com.mongodb.client.MongoDatabase;
import com.mongodb.client.model.Filters;
import com.mongodb.client.model.Updates;
import com.mongodb.client.result.DeleteResult;
import com.mongodb.client.result.InsertOneResult;
import com.mongodb.client.result.UpdateResult;
import org.bson.Document;
import org.bson.types.ObjectId;

public class MongoDBExample {
    public static void main(String[] args) {
        // Conectar a MongoDB
        MongoClient mongoClient = MongoClients.create("mongodb://localhost:27017");
        
        // Seleccionar base de datos y colección
        MongoDatabase database = mongoClient.getDatabase("miBaseDeDatos");
        MongoCollection<Document> usuarios = database.getCollection("usuarios");
        
        try {
            // Insertar un documento
            Document nuevoUsuario = new Document()
                    .append("nombre", "Sofía")
                    .append("email", "sofia@ejemplo.com")
                    .append("edad", 25);
            
            InsertOneResult insertResult = usuarios.insertOne(nuevoUsuario);
            ObjectId id = insertResult.getInsertedId().asObjectId().getValue();
            System.out.println("ID del documento insertado: " + id);
            
            // Buscar documentos
            System.out.println("Usuarios con más de 20 años:");
            usuarios.find(Filters.gt("edad", 20)).forEach(doc -> {
                System.out.println(doc.toJson());
            });
            
            // Actualizar un documento
            UpdateResult updateResult = usuarios.updateOne(
                    Filters.eq("nombre", "Sofía"),
                    Updates.set("edad", 26)
            );
            System.out.println("Documentos actualizados: " + updateResult.getModifiedCount());
            
            // Eliminar un documento
            DeleteResult deleteResult = usuarios.deleteOne(Filters.eq("_id", id));
            System.out.println("Documentos eliminados: " + deleteResult.getDeletedCount());
            
        } finally {
            // Cerrar la conexión
            mongoClient.close();
        }
    }
}
```

#### Ejemplo con Spring Boot
```java
// Modelo
@Document(collection = "usuarios")
public class Usuario {
    @Id
    private String id;
    private String nombre;
    private String email;
    private int edad;
    
    // Constructores, getters y setters
    public Usuario() {}
    
    public Usuario(String nombre, String email, int edad) {
        this.nombre = nombre;
        this.email = email;
        this.edad = edad;
    }
    
    // Getters y setters
    public String getId() { return id; }
    public void setId(String id) { this.id = id; }
    
    public String getNombre() { return nombre; }
    public void setNombre(String nombre) { this.nombre = nombre; }
    
    public String getEmail() { return email; }
    public void setEmail(String email) { this.email = email; }
    
    public int getEdad() { return edad; }
    public void setEdad(int edad) { this.edad = edad; }
}

// Repositorio
public interface UsuarioRepository extends MongoRepository<Usuario, String> {
    List<Usuario> findByEdadGreaterThan(int edad);
    Usuario findByEmail(String email);
}

// Controlador
@RestController
@RequestMapping("/usuarios")
public class UsuarioController {
    
    @Autowired
    private UsuarioRepository usuarioRepository;
    
    @GetMapping
    public List<Usuario> getAllUsuarios() {
        return usuarioRepository.findAll();
    }
    
    @GetMapping("/{id}")
    public ResponseEntity<Usuario> getUsuarioById(@PathVariable String id) {
        return usuarioRepository.findById(id)
                .map(ResponseEntity::ok)
                .orElse(ResponseEntity.notFound().build());
    }
    
    @PostMapping
    public ResponseEntity<Usuario> createUsuario(@RequestBody Usuario usuario) {
        Usuario nuevoUsuario = usuarioRepository.save(usuario);
        return ResponseEntity.status(HttpStatus.CREATED).body(nuevoUsuario);
    }
    
    @PutMapping("/{id}")
    public ResponseEntity<Usuario> updateUsuario(@PathVariable String id, @RequestBody Usuario usuario) {
        return usuarioRepository.findById(id)
                .map(usuarioExistente -> {
                    usuario.setId(id);
                    return ResponseEntity.ok(usuarioRepository.save(usuario));
                })
                .orElse(ResponseEntity.notFound().build());
    }
    
    @DeleteMapping("/{id}")
    public ResponseEntity<Void> deleteUsuario(@PathVariable String id) {
        return usuarioRepository.findById(id)
                .map(usuario -> {
                    usuarioRepository.delete(usuario);
                    return ResponseEntity.noContent().<Void>build();
                })
                .orElse(ResponseEntity.notFound().build());
    }
    
    @GetMapping("/filtro/edad/{edad}")
    public List<Usuario> getUsuariosByEdad(@PathVariable int edad) {
        return usuarioRepository.findByEdadGreaterThan(edad);
    }
}
```

### PHP

#### Instalación
```bash
composer require mongodb/mongodb
```

#### Conexión y operaciones básicas
```php
<?php
require 'vendor/autoload.php';

// Conectar a MongoDB
$client = new MongoDB\Client("mongodb://localhost:27017");

// Seleccionar base de datos y colección
$database = $client->miBaseDeDatos;
$usuarios = $database->usuarios;

// Insertar un documento
$resultado = $usuarios->insertOne([
    'nombre' => 'Laura',
    'email' => 'laura@ejemplo.com',
    'edad' => 29
]);
echo "ID del documento insertado: " . $resultado->getInsertedId() . "\n";

// Buscar documentos
$cursor = $usuarios->find(['edad' => ['$gt' => 25]]);
echo "Usuarios con más de 25 años:\n";
foreach ($cursor as $usuario) {
    echo json_encode($usuario) . "\n";
}

// Buscar un documento específico
$usuario = $usuarios->findOne(['_id' => $resultado->getInsertedId()]);
echo "Usuario encontrado: " . json_encode($usuario) . "\n";

// Actualizar un documento
$actualizacion = $usuarios->updateOne(
    ['nombre' => 'Laura'],
    ['$set' => ['edad' => 30]]
);
echo "Documentos actualizados: " . $actualizacion->getModifiedCount() . "\n";

// Eliminar un documento
$eliminacion = $usuarios->deleteOne(['nombre' => 'Laura']);
echo "Documentos eliminados: " . $eliminacion->getDeletedCount() . "\n";
?>
```

#### Ejemplo con Laravel
```php
// Modelo (app/Models/Usuario.php)
<?php
namespace App\Models;

use Jenssegers\Mongodb\Eloquent\Model;

class Usuario extends Model
{
    protected $connection = 'mongodb';
    protected $collection = 'usuarios';
    protected $fillable = ['nombre', 'email', 'edad'];
}

// Controlador (app/Http/Controllers/UsuarioController.php)
<?php
namespace App\Http\Controllers;

use App\Models\Usuario;
use Illuminate\Http\Request;

class UsuarioController extends Controller
{
    public function index()
    {
        $usuarios = Usuario::all();
        return response()->json($usuarios);
    }
    
    public function show($id)
    {
        $usuario = Usuario::find($id);
        if (!$usuario) {
            return response()->json(['error' => 'Usuario no encontrado'], 404);
        }
        return response()->json($usuario);
    }
    
    public function store(Request $request)
    {
        $request->validate([
            'nombre' => 'required|string',
            'email' => 'required|email|unique:mongodb.usuarios',
            'edad' => 'required|integer|min:18'
        ]);
        
        $usuario = Usuario::create($request->all());
        return response()->json($usuario, 201);
    }
    
    public function update(Request $request, $id)
    {
        $usuario = Usuario::find($id);
        if (!$usuario) {
            return response()->json(['error' => 'Usuario no encontrado'], 404);
        }
        
        $request->validate([
            'nombre' => 'string',
            'email' => 'email|unique:mongodb.usuarios,email,' . $id,
            'edad' => 'integer|min:18'
        ]);
        
        $usuario->update($request->all());
        return response()->json($usuario);
    }
    
    public function destroy($id)
    {
        $usuario = Usuario::find($id);
        if (!$usuario) {
            return response()->json(['error' => 'Usuario no encontrado'], 404);
        }
        
        $usuario->delete();
        return response()->json(['mensaje' => 'Usuario eliminado correctamente']);
    }
    
    public function findByAge($edad)
    {
        $usuarios = Usuario::where('edad', '>', $edad)->get();
        return response()->json($
use miBaseDeDatos
```
> Nota: La base de datos no se crea físicamente hasta que se inserta al menos un documento.

#### Mostrar bases de datos existentes
```javascript
show dbs
```

#### Verificar la base de datos actual
```javascript
db
```

#### Eliminar una base de datos
```javascript
use miBaseDeDatos
db.dropDatabase()
```

### Colecciones

#### Crear una colección
```javascript
// Creación explícita
db.createCollection("usuarios")

// Creación implícita (se crea al insertar el primer documento)
db.productos.insertOne({ nombre: "Laptop", precio: 999.99 })
```

#### Crear una colección con opciones
```javascript
db.createCollection("registros", {
  capped: true,         // Colección limitada (tamaño fijo)
  size: 10000000,       // Tamaño máximo en bytes
  max: 1000,            // Número máximo de documentos
  validator: {          // Reglas de validación para documentos
    $jsonSchema: {
      bsonType: "object",
      required: ["nombre", "email"],
      properties: {
        nombre: {
          bsonType: "string",
          description: "El nombre es obligatorio y debe ser un string"
        },
        email: {
          bsonType: "string",
          pattern: "^[a-zA-Z0-9._%+-]+@[a-zA-Z0-9.-]+\\.[a-zA-Z]{2,}$",
          description: "El email es obligatorio y debe tener un formato válido"
        },
        edad: {
          bsonType: "int",
          minimum: 18,
          maximum: 99,
          description: "La edad debe ser un entero entre 18 y 99"
        }
      }
    }
  },
  validationLevel: "moderate",  // strict | moderate | off
  validationAction: "error"     // error | warn
})
```

#### Mostrar colecciones
```javascript
show collections
```

#### Renombrar una colección
```javascript
db.usuarios.renameCollection("clientes")
```

#### Eliminar una colección
```javascript
db.usuarios.drop()
```

### Esquemas y Validación

MongoDB es una base de datos sin esquema por defecto, pero ofrece validación de esquemas para imponer reglas.

#### Añadir validación a una colección existente
```javascript
db.runCommand({
  collMod: "usuarios",
  validator: {
    $jsonSchema: {
      bsonType: "object",
      required: ["nombre", "email"],
      properties: {
        nombre: {
          bsonType: "string",
          description: "El nombre es obligatorio y debe ser un string"
        },
        email: {
          bsonType: "string",
          pattern: "^[a-zA-Z0-9._%+-]+@[a-zA-Z0-9.-]+\\.[a-zA-Z]{2,}$",
          description: "El email es obligatorio y debe tener un formato válido"
        }
      }
    }
  },
  validationLevel: "moderate",
  validationAction: "error"
})
```

#### Modificar el nivel de validación
```javascript
db.runCommand({
  collMod: "usuarios",
  validationLevel: "off"  // Desactivar la validación temporalmente
})
```

### Vistas

MongoDB soporta vistas que son colecciones de solo lectura.

#### Crear una vista
```javascript
db.createView(
  "usuariosActivos",     // Nombre de la vista
  "usuarios",            // Colección origen
  [
    { $match: { activo: true } },
    { $project: { _id: 1, nombre: 1, email: 1 } }
  ]
)
```

#### Eliminar una vista
```javascript
db.usuariosActivos.drop()
```

### Transacciones

MongoDB soporta transacciones ACID multi-documento desde la versión 4.0.

#### Ejemplo de transacción
```javascript
// Iniciar sesión
const session = db.getMongo().startSession();
session.startTransaction();

try {
  // Operaciones dentro de la transacción
  const cuentasCollection = session.getDatabase("banco").cuentas;
  
  cuentasCollection.updateOne(
    { numero: "123456" },
    { $inc: { saldo: -100 } }
  );
  
  cuentasCollection.updateOne(
    { numero: "789012" },
    { $inc: { saldo: 100 } }
  );
  
  // Si todo va bien, confirmar la transacción
  session.commitTransaction();
} catch (error) {
  // Si hay un error, deshacer la transacción
  session.abortTransaction();
  print("Error en la transacción: " + error);
} finally {
  // Cerrar la sesión
  session.endSession();
}
```

## Operaciones DML (Data Manipulation Language)

Las operaciones DML en MongoDB se refieren a las operaciones que manipulan los datos dentro de las colecciones, como insertar, consultar, actualizar y eliminar documentos.

### Inserción de documentos

#### Insertar un solo documento
```javascript
db.usuarios.insertOne({
  nombre: "Ana",
  apellido: "García",
  edad: 28,
  email: "ana.garcia@ejemplo.com"
})
```

#### Insertar múltiples documentos
```javascript
db.usuarios.insertMany([
  {
    nombre: "Carlos",
    apellido: "Rodríguez",
    edad: 35,
    email: "carlos.rodriguez@ejemplo.com"
  },
  {
    nombre: "María",
    apellido: "López",
    edad: 42,
    email: "maria.lopez@ejemplo.com"
  }
])
```

#### Insertar o actualizar (upsert)
```javascript
db.usuarios.updateOne(
  { email: "pedro.martinez@ejemplo.com" },
  {
    $set: {
      nombre: "Pedro",
      apellido: "Martínez",
      edad: 31
    }
  },
  { upsert: true }
)
```

### Consulta de documentos

#### Consultar todos los documentos
```javascript
db.usuarios.find()
```

#### Consultar con formato legible
```javascript
db.usuarios.find().pretty()
```

#### Consultar con filtros
```javascript
// Igualdad
db.usuarios.find({ edad: 28 })

// Operadores de comparación
db.usuarios.find({ edad: { $gt: 30 } })  // mayor que
db.usuarios.find({ edad: { $lt: 40 } })  // menor que
db.usuarios.find({ edad: { $gte: 30 } }) // mayor o igual que
db.usuarios.find({ edad: { $lte: 40 } }) // menor o igual que
db.usuarios.find({ edad: { $ne: 35 } })  // diferente de
db.usuarios.find({ edad: { $in: [28, 35, 42] } }) // dentro de la lista
db.usuarios.find({ edad: { $nin: [28, 35, 42] } }) // fuera de la lista

// Operadores lógicos
db.usuarios.find({ $and: [{ edad: { $gt: 30 } }, { edad: { $lt: 40 } }] })
db.usuarios.find({ $or: [{ edad: { $lt: 30 } }, { edad: { $gt: 40 } }] })
db.usuarios.find({ edad: { $not: { $gt: 30 } } })
db.usuarios.find({ $nor: [{ edad: 28 }, { edad: 35 }] })

// Consulta por campos anidados
db.usuarios.find({ "direccion.ciudad": "Madrid" })

// Consulta por elementos de un array
db.usuarios.find({ telefonos: "+34600123456" })
```

#### Proyección (seleccionar campos)
```javascript
// Incluir solo ciertos campos
db.usuarios.find({}, { nombre: 1, email: 1 })

// Excluir ciertos campos
db.usuarios.find({}, { direccion: 0, telefonos: 0 })

// Proyección con campos anidados
db.usuarios.find({}, { "direccion.ciudad": 1, "direccion.codigo_postal": 1 })
```

#### Ordenar resultados
```javascript
// Ascendente (1)
db.usuarios.find().sort({ edad: 1 })

// Descendente (-1)
db.usuarios.find().sort({ apellido: -1 })

// Múltiples campos
db.usuarios.find().sort({ apellido: 1, nombre: 1 })
```

#### Limitar resultados
```javascript
// Limitar a N documentos
db.usuarios.find().limit(5)

// Saltar N documentos
db.usuarios.find().skip(10)

// Paginación
db.usuarios.find().skip(10).limit(5)  // página 3 (con 5 por página)
```

#### Contar documentos
```javascript
db.usuarios.countDocuments({ edad: { $gt: 30 } })
```

#### Documentos distintos
```javascript
db.usuarios.distinct("direccion.ciudad")
```

### Actualización de documentos

#### Actualizar un documento
```javascript
db.usuarios.updateOne(
  { email: "ana.garcia@ejemplo.com" },  // filtro
  {
    $set: { edad: 29 }  // operador de actualización
  }
)
```

#### Actualizar múltiples documentos
```javascript
db.usuarios.updateMany(
  { "direccion.ciudad": "Madrid" },
  {
    $set: { "direccion.region": "Comunidad de Madrid" }
  }
)
```

#### Operadores de actualización
```javascript
// $set - Establecer valores
db.usuarios.updateOne(
  { _id: ObjectId("60a5e8380c314f9f644a0416") },
  { $set: { edad: 31, activo: true } }
)

// $unset - Eliminar campos
db.usuarios.updateOne(
  { _id: ObjectId("60a5e8380c314f9f644a0416") },
  { $unset: { telefono_fijo: "" } }
)

// $inc - Incrementar valores numéricos
db.usuarios.updateOne(
  { _id: ObjectId("60a5e8380c314f9f644a0416") },
  { $inc: { edad: 1, visitas: 5 } }
)

// $mul - Multiplicar valores numéricos
db.usuarios.updateOne(
  { _id: ObjectId("60a5e8380c314f9f644a0416") },
  { $mul: { puntos: 2 } }
)

// $min - Actualizar si el nuevo valor es menor
db.usuarios.updateOne(
  { _id: ObjectId("60a5e8380c314f9f644a0416") },
  { $min: { edad: 25 } }  // Solo actualizará si la edad actual > 25
)

// $max - Actualizar si el nuevo valor es mayor
db.usuarios.updateOne(
  { _id: ObjectId("60a5e8380c314f9f644a0416") },
  { $max: { puntuacion: 95 } }  // Solo actualizará si la puntuación actual < 95
)

// $rename - Renombrar campos
db.usuarios.updateMany(
  {},
  { $rename: { "telefono": "telefono_principal" } }
)

// Operadores para arrays
// $push - Añadir elementos a un array
db.usuarios.updateOne(
  { _id: ObjectId("60a5e8380c314f9f644a0416") },
  { $push: { telefonos: "+34600123457" } }
)

// $addToSet - Añadir elementos a un array sin duplicados
db.usuarios.updateOne(
  { _id: ObjectId("60a5e8380c314f9f644a0416") },
  { $addToSet: { categorias: "premium" } }
)

// $pull - Eliminar elementos de un array que coincidan
db.usuarios.updateOne(
  { _id: ObjectId("60a5e8380c314f9f644a0416") },
  { $pull: { telefonos: "+34600123456" } }
)

// $pop - Eliminar el primer (-1) o último (1) elemento de un array
db.usuarios.updateOne(
  { _id: ObjectId("60a5e8380c314f9f644a0416") },
  { $pop: { telefonos: 1 } }  // Elimina el último teléfono
)
```

#### Reemplazar un documento completo
```javascript
db.usuarios.replaceOne(
  { _id: ObjectId("60a5e8380c314f9f644a0416") },
  {
    nombre: "Ana María",
    apellido: "García López",
    email: "ana.garcia@ejemplo.com",
    edad: 29
  }
)
```

### Eliminación de documentos

#### Eliminar un documento
```javascript
db.usuarios.deleteOne({ email: "ana.garcia@ejemplo.com" })
```

#### Eliminar múltiples documentos
```javascript
db.usuarios.deleteMany({ activo: false })
```

#### Eliminar todos los documentos
```javascript
db.usuarios.deleteMany({})
```

#### Eliminar y devolver el documento
```javascript
db.usuarios.findOneAndDelete({ email: "ana.garcia@ejemplo.com" })
```

### Operaciones atómicas

MongoDB proporciona métodos para operaciones atómicas de "buscar y modificar".

#### Buscar y actualizar
```javascript
db.usuarios.findOneAndUpdate(
  { _id: ObjectId("60a5e8380c314f9f644a0416") },
  { $inc: { contador: 1 } },
  {
    returnDocument: "after",  // Devolver el documento actualizado
    upsert: true             // Crear si no existe
  }
)
```

#### Buscar y reemplazar
```javascript
db.usuarios.findOneAndReplace(
  { email: "ana.garcia@ejemplo.com" },
  {
    nombre: "Ana",
    apellido: "García",
    email: "ana.garcia@ejemplo.com",
    edad: 30
  },
  { returnDocument: "after" }
)
```

## Consultas Avanzadas

### Operadores de consulta avanzados

#### Operadores de elementos
```javascript
// $exists - Comprobar si existe un campo
db.usuarios.find({ telefono: { $exists: true } })

// $type - Filtrar por tipo de datos
db.usuarios.find({ edad: { $type: "int" } })
db.usuarios.find({ descripcion: { $type: "string" } })
```

#### Operadores de evaluación
```javascript
// $regex - Expresiones regulares
db.usuarios.find({ email: { $regex: /gmail\.com$/ } })
db.usuarios.find({ nombre: { $regex: /^A/, $options: "i" } }) // Insensible a mayúsculas/minúsculas

// $expr - Expresiones de agregación en consultas
db.ventas.find({ $expr: { $gt: ["$ingresos", "$gastos"] } })

// $mod - Operador módulo
db.numeros.find({ valor: { $mod: [5, 0] } }) // Valores divisibles por 5
```

#### Operadores de arrays
```javascript
// $all - Coincide con arrays que contienen todos los elementos
db.productos.find({ etiquetas: { $all: ["oferta", "electrónica"] } })

// $elemMatch - Coincide con documentos que tienen al menos un elemento que cumple todas las condiciones
db.estudiantes.find({
  calificaciones: { $elemMatch: { $gte: 90, $lt: 100 } }
})

// $size - Coincide con arrays de tamaño específico
db.usuarios.find({ telefonos: { $size: 2 } })
```

#### Operadores geoespaciales
```javascript
// Crear un índice geoespacial
db.lugares.createIndex({ ubicacion: "2dsphere" })

// Buscar lugares cercanos a un punto
db.lugares.find({
  ubicacion: {
    $near: {
      $geometry: {
        type: "Point",
        coordinates: [-3.7037902, 40.4167754]  // [longitud, latitud]
      },
      $maxDistance: 1000  // En metros
    }
  }
})

// Buscar lugares dentro de un área
db.lugares.find({
  ubicacion: {
    $geoWithin: {
      $geometry: {
        type: "Polygon",
        coordinates: [
          [
            [-3.7037902, 40.4167754],
            [-3.7037902, 40.4267754],
            [-3.6937902, 40.4267754],
            [-3.6937902, 40.4167754],
            [-3.7037902, 40.4167754]
          ]
        ]
      }
    }
  }
})
```

### Búsqueda de texto completo
```javascript
// Crear un índice de texto
db.articulos.createIndex({ titulo: "text", contenido: "text" })

// Búsqueda simple de texto
db.articulos.find({ $text: { $search: "mongodb nosql" } })

// Búsqueda con puntuación de relevancia
db.articulos.find(
  { $text: { $search: "mongodb nosql" } },
  { score: { $meta: "textScore" } }
).sort({ score: { $meta: "textScore" } })
```

La puntuación (score) representa la relevancia de un documento para una consulta de texto específica. Los documentos con mayor puntuación son más relevantes. MongoDB calcula esta puntuación basándose en varios factores como la frecuencia de los términos, el peso de los campos y otros factores estadísticos.

#### Modificar la relevancia con pesos
```javascript
// Crear un índice de texto con pesos específicos para cada campo
db.articulos.createIndex(
  { titulo: "text", contenido: "text", descripcion: "text" },
  {
    weights: {
      titulo: 10,      // Mayor importancia
      contenido: 5,    // Importancia media
      descripcion: 1   // Menor importancia
    },
    name: "IndiceTextoConPesos"
  }
)
```

### Consultas con expresiones regulares
```javascript
// Búsqueda con expresión regular
db.usuarios.find({ email: /gmail\.com$/ })

// Búsqueda insensible a mayúsculas/minúsculas
db.usuarios.find({ nombre: /^a/i })
```

## Indexación en MongoDB

Los índices en MongoDB son estructuras de datos especiales que almacenan una pequeña porción de los datos de la colección de forma ordenada, facilitando operaciones de búsqueda eficientes.

### Tipos de índices

#### Índice simple
```javascript
// Crear un índice simple
db.usuarios.createIndex({ email: 1 })  // 1 para orden ascendente, -1 para descendente
```

#### Índice compuesto
```javascript
// Crear un índice compuesto
db.usuarios.createIndex({ apellido: 1, nombre: 1 })
```

#### Índice único
```javascript
// Crear un índice único
db.usuarios.createIndex({ email: 1 }, { unique: true })
```

#### Índice sparse
```javascript
// Crear un índice sparse (solo indexa documentos que tienen el campo)
db.usuarios.createIndex({ telefono: 1 }, { sparse: true })
```

#### Índice TTL (Time-To-Live)
```javascript
// Crear un índice TTL (elimina documentos después de un tiempo)
db.sesiones.createIndex({ fecha_ultimo_acceso: 1 }, { expireAfterSeconds: 3600 })
```

#### Índice parcial
```javascript
// Crear un índice parcial (solo indexa documentos que cumplen la condición)
db.usuarios.createIndex(
  { email: 1 },
  { 
    partialFilterExpression: { activo: true }
  }
)
```

#### Índice de texto
```javascript
// Crear un índice de texto (ya visto anteriormente)
db.articulos.createIndex({ titulo: "text", contenido: "text" })
```

#### Índice geoespacial
```javascript
// Crear un índice geoespacial 2dsphere
db.lugares.createIndex({ ubicacion: "2dsphere" })

// Crear un índice geoespacial 2d (para coordenadas planas)
db.mapa.createIndex({ coordenadas: "2d" })
```

#### Índice hashed
```javascript
// Crear un índice hashed (útil para sharding)
db.usuarios.createIndex({ _id: "hashed" })
```

### Gestión de índices

#### Listar índices
```javascript
db.usuarios.getIndexes()
```

#### Eliminar un índice
```javascript
// Por nombre
db.usuarios.dropIndex("nombre_del_indice")

// Por especificación
db.usuarios.dropIndex({ email: 1 })
```

#### Eliminar todos los índices
```javascript
db.usuarios.dropIndexes()
```

#### Reconstruir índices
```javascript
db.usuarios.reIndex()
```

### Análisis de rendimiento de índices

#### Explain
```javascript
// Analizar el plan de ejecución de una consulta
db.usuarios.find({ edad: { $gt: 30 } }).explain()

// Análisis detallado
db.usuarios.find({ edad: { $gt: 30 } }).explain("executionStats")

// Análisis completo
db.usuarios.find({ edad: { $gt: 30 } }).explain("allPlansExecution")
```

#### Estadísticas de índices
```javascript
db.usuarios.stats()
```

#### Monitorizar el uso de índices
```javascript
db.usuarios.aggregate([
  { $indexStats: {} }
])
```

## Framework de Agregación

El framework de agregación de MongoDB proporciona un medio para realizar operaciones de procesamiento de datos complejas, similar a las consultas GROUP BY en SQL pero con muchas más capacidades.

### Etapas de agregación

#### $match - Filtrar documentos
```javascript
db.ventas.aggregate([
  { $match: { estado: "completado" } }
])
```

#### $group - Agrupar documentos
```javascript
db.ventas.aggregate([
  { 
    $group: { 
      _id: "$region",
      totalVentas: { $sum: "$importe" },
      cantidadVentas: { $sum: 1 },
      importePromedio: { $avg: "$importe" },
      importeMaximo: { $max: "$importe" },
      importeMinimo: { $min: "$importe" }
    } 
  }
])
```

#### $project - Proyectar campos
```javascript
db.usuarios.aggregate([
  {
    $project: {
      nombreCompleto: { $concat: ["$nombre", " ", "$apellido"] },
      edad: 1,
      _id: 0
    }
  }
])
```

#### $sort - Ordenar documentos
```javascript
db.ventas.aggregate([
  { $sort: { fecha: -1, importe: -1 } }
])
```

#### $limit - Limitar documentos
```javascript
db.ventas.aggregate([
  { $limit: 10 }
])
```

#### $skip - Saltar documentos
```javascript
db.ventas.aggregate([
  { $skip: 20 }
])
```

#### $unwind - Desagrupar arrays
```javascript
db.productos.aggregate([
  { $unwind: "$categorias" }
])
```

#### $lookup - Realizar "joins" con otras colecciones
```javascript
db.pedidos.aggregate([
  {
    $lookup: {
      from: "clientes",
      localField: "cliente_id",
      foreignField: "_id",
      as: "cliente_info"
    }
  }
])
```

#### $addFields - Añadir nuevos campos
```javascript
db.ventas.aggregate([
  {
    $addFields: {
      impuestos: { $multiply: ["$importe", 0.21] },
      importeTotal: { $multiply: ["$importe", 1.21] }
    }
  }
])
```

#### $count - Contar documentos
```javascript
db.usuarios.aggregate([
  { $match: { activo: true } },
  { $count: "usuarios_activos" }
])
```

#### $facet - Múltiples agregaciones en paralelo
```javascript
db.ventas.aggregate([
  {
    $facet: {
      "por_region": [
        { $group: { _id: "$region", total: { $sum: "$importe" } } }
      ],
      "por_producto": [
        { $group: { _id: "$producto", total: { $sum: "$importe" } } }
      ],
      "total_general": [
        { $group: { _id: null, total: { $sum: "$importe" } } }
      ]
    }
  }
])
```

### Operadores de agregación

#### Operadores aritméticos
```javascript
db.productos.aggregate([
  {
    $project: {
      nombre: 1,
      precio: 1,
      precio_con_descuento: { $subtract: ["$precio", { $multiply: ["$precio", 0.1] }] }
    }
  }
])
```

#### Operadores de fecha
```javascript
db.ventas.aggregate([
  {
    $project: {
      año: { $year: "$fecha" },
      mes: { $month: "$fecha" },
      dia: { $dayOfMonth: "$fecha" },
      diaSemana: { $dayOfWeek: "$fecha" }
    }
  }
])
```

#### Operadores de cadena
```javascript
db.usuarios.aggregate([
  {
    $project: {
      nombre: { $toUpper: "$nombre" },
      apellido: { $toLower: "$apellido" },
      iniciales: { 
        $concat: [
          { $substr: ["$nombre", 0, 1] },
          { $substr: ["$apellido", 0, 1] }
        ]
      }
    }
  }
])
```

#### Operadores condicionales
```javascript
db.empleados.aggregate([
  {
    $project: {
      nombre: 1,
      salario: 1,
      categoria: {
        $cond: {
          if: { $gte: ["$salario", 50000] },
          then: "Senior",
          else: "Junior"
        }
      }
    }
  }
])
```

#### Operadores de acumulación
```javascript
db.ventas.aggregate([
  {
    $group: {
      _id: "$region",
      ventas_array: { $push: "$importe" },
      productos_array: { $addToSet: "$producto" }
    }
  }
])
```

### Optimización de agregaciones

#### Uso de índices
```javascript
// Asegúrate de tener índices para los campos usados en $match y $sort
db.ventas.createIndex({ fecha: 1, region: 1 })

// Luego usa esos campos en etapas tempranas de la agregación
db.ventas.aggregate([
  { $match: { fecha: { $gte: ISODate("2023-01-01") } } },
  { $sort: { region: 1, fecha: 1 } },
  // Resto de la agregación
])
```

#### Explicar planes de agregación
```javascript
db.ventas.explain("executionStats").aggregate([
  // Etapas de agregación
])
```

#### Tamaño del lote
```javascript
// Especificar el tamaño del lote para optimizar memoria
db.ventas.aggregate(
  [
    // Etapas de agregación
  ],
  { allowDiskUse: true, batchSize: 100 }
)
```

## MongoDB Atlas: La Nube de MongoDB

MongoDB Atlas es un servicio de base de datos como servicio (DBaaS) que proporciona MongoDB en la nube con características avanzadas de seguridad, escalabilidad y monitorización.

### Características principales

- **Aprovisionamiento automatizado**: Clusters de MongoDB en minutos
- **Alta disponibilidad**: Replicación automática y failover
- **Escalabilidad**: Escalar vertical u horizontalmente según sea necesario
- **Seguridad**: Cifrado en reposo y en tránsito, autenticación avanzada
- **Copias de seguridad**: Backups continuos y point-in-time recovery
- **Monitorización**: Paneles de rendimiento y alertas
- **Global**: Clusters distribuidos globalmente
- **Búsqueda de texto completo**: Atlas Search basado en Lucene
- **Análisis de datos**: Atlas Data Lake y Charts
- **Disparadores y funciones**: Atlas Triggers y Functions

### Configuración de MongoDB Atlas

1. **Crear una cuenta**:
   - Visita [MongoDB Atlas](https://www.mongodb.com/cloud/atlas)
   - Regístrate para obtener una cuenta gratuita

2. **Crear un cluster**:
   - Selecciona un proveedor de nube (AWS, Azure, GCP)
   - Elige una región geográfica
   - Selecciona el tamaño del cluster (incluye nivel gratuito)
   - Configura opciones adicionales

3. **Configurar la seguridad**:
   - Crea un usuario de base de datos
   - Configura la lista blanca de IP
   - Configura la autenticación

4. **Conectar a tu cluster**:
   - Obtén la cadena de conexión
   - Conéctate con MongoDB Compass o tu aplicación

### Atlas Search

Atlas Search integra capacidades de búsqueda de texto completo en MongoDB Atlas utilizando Apache Lucene.

#### Crear un índice de búsqueda
```javascript
// Desde la interfaz de Atlas:
// 1. Selecciona tu cluster
// 2. Ve a la pestaña "Search"
// 3. Haz clic en "Create Search Index"
// 4. Selecciona una configuración (Mapping o JSON)

// Ejemplo de configuración JSON para un índice de búsqueda
{
  "mappings": {
    "dynamic": true,
    "fields": {
      "titulo": {
        "type": "string",
        "analyzer": "lucene.spanish"
      },
      "contenido": {
        "type": "string",
        "analyzer": "lucene.spanish"
      },
      "etiquetas": {
        "type": "string",
        "analyzer": "keyword"
      }
    }
  }
}
```

#### Realizar búsquedas con Atlas Search
```javascript
db.articulos.aggregate([
  {
    $search: {
      "text": {
        "query": "mongodb base de datos",
        "path": ["titulo", "contenido"],
        "fuzzy": {
          "maxEdits": 1
        }
      }
    }
  },
  {
    $project: {
      titulo: 1,
      resumen: 1,
      score: { $meta: "searchScore" }
    }
  },
  {
    $sort: {
      score: -1
    }
  }
])
```

### Atlas Data Lake

Atlas Data Lake permite consultar datos en Amazon S3 utilizando el lenguaje de consulta de MongoDB.

```javascript
// Consultar datos en un Data Lake
db.getSiblingDB("DataLake").collection.aggregate([
  {
    $match: {
      fecha: { $gte: ISODate("2023-01-01") }
    }
  },
  {
    $group: {
      _id: "$region",
      totalVentas: { $sum: "$importe" }
    }
  }
])
```

### Atlas Charts

Atlas Charts proporciona visualizaciones de datos directamente desde MongoDB Atlas.

Pasos para crear un gráfico:
1. Selecciona tu fuente de datos
2. Elige un tipo de gráfico (barras, líneas, circular, etc.)
3. Configura los campos para ejes X e Y
4. Personaliza el estilo y opciones
5. Guarda y comparte

## Seguridad en MongoDB

La seguridad es un aspecto crucial en la administración de MongoDB. Incluye autenticación, autorización, cifrado y auditoría.

### Autenticación

#### Habilitar autenticación
```javascript
// En el archivo de configuración (mongod.conf)
security:
  authorization: enabled
```

#### Crear usuario administrador
```javascript
db.createUser({
  user: "adminUser",
  pwd: "contraseñaSegura",
  roles: [{ role: "userAdminAnyDatabase", db: "admin" }]
})
```

#### Crear usuario de aplicación
```javascript
db.createUser({
  user: "appUser",
  pwd: "contraseñaApp",
  roles: [
    { role: "readWrite", db: "miBaseDeDatos" },
    { role: "read", db: "otraBaseDeDatos" }
  ]
})
```

#### Autenticarse
```javascript
// Desde la línea de comandos
mongosh --port 27017 -u "adminUser" -p "contraseñaSegura" --authenticationDatabase "admin"

// Desde la consola de MongoDB
db.auth("adminUser", "contraseñaSegura")
```

### Roles y privilegios

MongoDB utiliza un sistema de roles para definir los privilegios de los usuarios.

#### Roles predefinidos
- **Database User Roles**: `read`, `readWrite`
- **Database Administration Roles**: `dbAdmin`, `dbOwner`, `userAdmin`
- **Cluster Administration Roles**: `clusterAdmin`, `clusterManager`, `clusterMonitor`, `hostManager`
- **Backup and Restoration Roles**: `backup`, `restore`
- **All-Database Roles**: `readAnyDatabase`, `readWriteAnyDatabase`, `userAdminAnyDatabase`, `dbAdminAnyDatabase`
- **Superuser Roles**: `root`

#### Crear un rol personalizado
```javascript
db.createRole({
  role: "myCustomRole",
  privileges: [
    {
      resource: { db: "miBaseDeDatos", collection: "usuarios" },
      actions: ["find", "update", "insert"]
    }
  ],
  roles: [
    { role: "read", db: "otraBaseDeDatos" }
  ]
})
```

#### Asignar roles a usuarios
```javascript
db.grantRolesToUser(
  "appUser",
  [{ role: "myCustomRole", db: "miBaseDeDatos" }]
)
```

### Cifrado

#### Cifrado en tránsito (TLS/SSL)
```javascript
// En el archivo de configuración (mongod.conf)
net:
  ssl:
    mode: requireSSL
    PEMKeyFile: /path/to/mongodb.pem
    CAFile: /path/to/ca.pem
```

#### Cifrado en reposo
MongoDB Enterprise incluye cifrado de datos en reposo con:
- **Cifrado a nivel de almacenamiento (WiredTiger)**
- **Cifrado a nivel de aplicación (Client-Side Field Level Encryption)**

```javascript
// Configurar cifrado a nivel de campo
const clientEncryption = new ClientEncryption(mongoClient, {
  keyVaultNamespace: 'encryption.__keyVault',
  kmsProviders: {
    local: {
      key: localMasterKey
    }
  }
});

// Crear claves de cifrado
const key = await clientEncryption.createDataKey('local');

// Cifrar datos
const encryptedValue = await clientEncryption.encrypt(
  sensitiveValue,
  {
    algorithm: 'AEAD_AES_256_CBC_HMAC_SHA_512-Deterministic',
    keyId: key
  }
);
```

### Auditoría

MongoDB Enterprise incluye capacidades de auditoría para rastrear actividades en la base de datos.

```javascript
// En el archivo de configuración (mongod.conf)
auditLog:
  destination: file
  format: JSON
  path: /var/log/mongodb/audit.json
  filter: '{ atype: { $in: ["authenticate", "createCollection", "dropCollection"] } }'
```

### Mejores prácticas de seguridad

1. **Deshabilitar el acceso público**: Configura el firewall para limitar el acceso.
2. **Usar autenticación y autorización**: Siempre habilita la autenticación y asigna permisos mínimos.
3. **Actualizar regularmente**: Mantén MongoDB actualizado con las últimas versiones de seguridad.
4. **Validar entradas**: Sanitiza todas las entradas para prevenir inyecciones.
5. **Auditar accesos**: Monitoriza y registra accesos y cambios importantes.
6. **Backups seguros**: Realiza copias de seguridad regulares y comprueba su restauración.
7. **Cifrado**: Utiliza cifrado en tránsito y en reposo.
8. **Configuración segura**: Revisa la configuración de MongoDB para eliminar vulnerabilidades.

## Replicación y Sharding

MongoDB proporciona alta disponibilidad mediante replicación y escalabilidad horizontal mediante sharding.

### Replicación

La replicación en MongoDB utiliza conjuntos de réplicas (replica sets) que consisten en múltiples servidores que mantienen copias de los mismos datos.

#### Configurar un conjunto de réplicas

1. **Iniciar las instancias de MongoDB**:
   ```bash
   # Nodo 1 (primario)
   mongod --replSet "rs0" --port 27017 --dbpath /data/db1

   # Nodo 2 (secundario)
   mongod --replSet "rs0" --port 27018 --dbpath /data/db2

   # Nodo 3 (secundario)
   mongod --replSet "rs0" --port 27019 --dbpath /data/db3
   ```

2. **Inicializar el conjunto de réplicas**:
   ```javascript
   // Conectar al primer nodo
   mongosh --port 27017
   
   // Configurar el conjunto de réplicas
   rs.initiate({
     _id: "rs0",
     members: [
       { _id: 0, host: "localhost:27017" },
       { _id: 1, host: "localhost:27018" },
       { _id: 2, host: "localhost:27019" }
     ]
   })
   ```

3. **Verificar el estado del conjunto de réplicas**:
   ```javascript
   rs.status()
   ```

#### Operaciones con conjuntos de réplicas

```javascript
// Ver la configuración actual
rs.conf()

// Agregar un nuevo miembro
rs.add("localhost:27020")

// Eliminar un miembro
rs.remove("localhost:27019")

// Cambiar la prioridad de un miembro
var cfg = rs.conf()
cfg.members[1].priority = 0.5
rs.reconfig(cfg)

// Forzar una elección de primario
rs.stepDown()

// Comprobar si una instancia es primaria
db.isMaster()
```

#### Opciones de lectura y escritura

```javascript
// Escritura con confirmación de mayoría
db.usuarios.insertOne(
  { nombre: "Ana" },
  { writeConcern: { w: "majority", wtimeout: 5000 } }
)

// Lectura desde primario
db.usuarios.find().readPref("primary")

// Lectura desde secundario
db.usuarios.find().readPref("secondary")

// Lectura desde el nodo más cercano
db.usuarios.find().readPref("nearest")
```

### Sharding

El sharding permite distribuir datos entre múltiples servidores para escalar horizontalmente.

#### Componentes de un cluster sharded
- **Shard**: Almacena una porción de los datos.
- **Config servers**: Almacenan metadatos sobre la distribución de datos.
- **Mongos**: Enruta consultas a los shards apropiados.

#### Configurar un cluster sharded

1. **Iniciar los servidores de configuración**:
   ```bash
   # Config server 1
   mongod --configsvr --replSet "configRS" --port 27019 --dbpath /data/configdb1
   
   # Config server 2
   mongod --configsvr --replSet "configRS" --port 27020 --dbpath /data/configdb2
   
   # Config server 3
   mongod --configsvr --replSet "configRS" --port 27021 --dbpath /data/configdb3
   ```

2. **Inicializar el conjunto de réplicas de configuración**:
   ```javascript
   mongosh --port 27019
   
   rs.initiate({
     _id: "configRS",
     configsvr: true,
     members: [
       { _id: 0, host: "localhost:27019" },
       { _id: 1, host: "localhost:27020" },
       { _id: 2, host: "localhost:27021" }
     ]
   })
   ```

3. **Iniciar los shards**:
   ```bash
   # Shard 1
   mongod --shardsvr --replSet "shard1RS" --port 27022 --dbpath /data/shard1
   
   # Shard 2
   mongod --shardsvr --replSet "shard2RS" --port 27023 --dbpath /data/shard2
   ```

4. **Configurar los conjuntos de réplicas para cada shard**.

5. **Iniciar el router mongos**:
   ```bash
   mongos --configdb configRS/localhost:27019,localhost:27020,localhost:27021 --port 27017
   ```

6. **Agregar shards al cluster**:
   ```javascript
   mongosh --port 27017
   
   sh.addShard("shard1RS/localhost:27022")
   sh.addShard("shard2RS/localhost:27023")
   ```

#### Configurar sharding para una base de datos

```javascript
// Habilitar sharding para una base de datos
sh.enableSharding("miBaseDeDatos")

// Crear un índice para la clave de sharding
db.miColeccion.createIndex({ campo_shard: 1 })

// Configurar sharding para una colección
sh.shardCollection(
  "miBaseDeDatos.miColeccion",
  { campo_shard: 1 }
)
```

#### Estrategias de sharding

1. **Sharding basado en rangos**:
   ```javascript
   sh.shardCollection(
     "miBaseDeDatos.miColeccion",
     { fecha: 1 }
   )
   ```

2. **Sharding basado en hashes**:
   ```javascript
   sh.shardCollection(
     "miBaseDeDatos.miColeccion",
     { _id: "hashed" }
   )
   ```

3. **Sharding zonal**:
   ```javascript
   // Definir zonas
   sh.addShardToZone("shard1", "us")
   sh.addShardToZone("shard2", "eu")
   
   // Asignar rangos a zonas
   sh.updateZoneKeyRange(
     "miBaseDeDatos.miColeccion",
     { region: "us" },
     { region: "us" },
     "us"
   )
   
   sh.updateZoneKeyRange(
     "miBaseDeDatos.miColeccion",
     { region: "eu" },
     { region: "eu" },
     "eu"
   )
   ```

#### Administración de sharding

```javascript
// Ver el estado del sharding
sh.status()

// Mover un chunk específico
sh.moveChunk(
  "miBaseDeDatos.miColeccion",
  { campo_shard: valor },
  "shard2"
)

// Balancear el cluster
sh.startBalancer()
sh.stopBalancer()
sh.isBalancerRunning()
```

## Optimización y Performance

Optimizar el rendimiento de MongoDB es crucial para aplicaciones escalables y eficientes.

### Monitorización del rendimiento

#### Estadísticas de la base de datos
```javascript
// Estadísticas generales
db.stats()

// Estadísticas de una colección
db.usuarios.stats()

// Estadísticas del servidor
db.serverStatus()
```

#### Monitorización de operaciones
```javascript
// Ver operaciones en curso
db.currentOp()

// Filtrar operaciones por tiempo
db.currentOp({
  "secs_running": { $gt: 5 }
})

// Matar una operación larga
db.killOp(opId)
```

#### Perfilado de operaciones
```javascript
// Habilitar el perfilador
db.setProfilingLevel(2)  // 0: apagado, 1: lentas, 2: todas

// Establecer umbral para operaciones lentas
db.setProfilingLevel(1, { slowms: 100 })

// Ver operaciones perfiladas
db.system.profile.find().sort({ ts: -1 }).limit(10)
```

### Optimización de consultas

#### Uso de índices apropiados
```javascript
// Crear índices para consultas frecuentes
db.usuarios.createIndex({ email: 1 })
db.productos.createIndex({ categoria: 1, precio: -1 })

// Verificar si una consulta usa índices
db.usuarios.find({ email: "ejemplo@gmail.com" }).explain("executionStats")
```

#### Consultas cubiertas
```javascript
// Consulta cubierta (usando solo índices)
db.usuarios.find(
  { edad: { $gt: 25 } },
  { _id: 0, nombre: 1, email: 1 }
).hint({ edad: 1, nombre: 1, email: 1 })
```

#### Proyección de campos
```javascript
// Proyectar solo los campos necesarios
db.productos.find(
  { categoria: "electrónica" },
  { nombre: 1, precio: 1, _id: 0 }
)
```

#### Limitar resultados
```javascript
// Limitar número de resultados
db.productos.find().limit(100)
```

#### Evitar ordenaciones sin índices
```javascript
// Crear índice para ordenación
db.productos.createIndex({ precio: -1 })

// Usar ordenación con índice
db.productos.find().sort({ precio: -1 })
```

### Optimización de escrituras

#### Escrituras por lotes
```javascript
// Inserción por lotes
db.productos.insertMany([
  { nombre: "Producto 1", precio: 10 },
  { nombre: "Producto 2", precio: 20 },
  // Más productos...
])

// Actualización por lotes
db.productos.updateMany(
  { categoria: "electrónica" },
  { $set: { descuento: 0.1 } }
)
```

#### Usar upserts cuando sea apropiado
```javascript
db.productos.updateOne(
  { sku: "ABC123" },
  { $set: { stock: 100 } },
  { upsert: true }
)
```

#### Reducir el tamaño de los documentos
```javascript
// Evitar campos innecesarios
// Usar tipos de datos apropiados (Int32 vs Double)
// Usar nombres de campo cortos
```

### Gestión de conexiones

```javascript
// Configuración en el lado del servidor (mongod.conf)
net:
  maxIncomingConnections: 65536

// Configuración del pool de conexiones en el cliente
MongoClient.connect(uri, {
  maxPoolSize: 100,
  minPoolSize: 5,
  maxIdleTimeMS: 30000
})
```

### Configuración de WiredTiger

```javascript