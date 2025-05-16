# Práctica de MongoDB: Lo Básico

## Introducción

Esta práctica está diseñada para aplicar los conceptos fundamentales de MongoDB aprendidos en el documento "MongoDB: De Cero a Héroe". Se enfoca en los aspectos esenciales: instalación, configuración, creación de una base de datos y operaciones CRUD básicas.

## Requisitos Previos

- Tener instalado MongoDB (versión 5.0 o superior)
- Tener instalado MongoDB Compass
- Conocimientos básicos de programación
- Haber leído el documento "MongoDB: De Cero a Héroe" proporcionado

---

# Parte 1: Instalación y Configuración

## Objetivo
- Instalar MongoDB en tu sistema
- Configurar MongoDB como servicio
- Conectarte a MongoDB usando la shell y MongoDB Compass

## Instrucciones

1. **Instalación**: 
   - Sigue las instrucciones de instalación para tu sistema operativo del documento "MongoDB: De Cero a Héroe".
   - Selecciona la instalación "Complete" para incluir MongoDB Compass.
   - Marca la opción "Install MongoDB as a Service" durante la instalación.

2. **Verificación de instalación**:
   - Abre una terminal o símbolo del sistema.
   - Ejecuta `mongosh` para conectarte a MongoDB.
   - Deberías ver algo similar a:
     ```
     Current Mongosh Log ID: 647b98f8d5d7e93a9299d123
     Connecting to:          mongodb://127.0.0.1:27017/?directConnection=true&serverSelectionTimeoutMS=2000&appName=mongosh+1.8.0
     Using MongoDB:          6.0.5
     ```

3. **Conectar con MongoDB Compass**:
   - Abre MongoDB Compass.
   - Conéctate a tu instancia local usando la URI: `mongodb://localhost:27017`.
   - Verifica que puedes ver el panel principal de MongoDB Compass.

## Entregable
- Una captura de pantalla mostrando la conexión exitosa a MongoDB con la shell (mongosh).
- Una captura de pantalla mostrando la conexión exitosa con MongoDB Compass.

# Parte 2: Creación de Base de Datos y Operaciones CRUD

## Objetivo
- Crear una base de datos y colecciones
- Realizar operaciones básicas de Crear, Leer, Actualizar y Eliminar documentos (CRUD)
- Utilizar consultas simples para recuperar información

## Instrucciones

### 1. Crear una Base de Datos de Tienda Online

```javascript
// Crear y usar una nueva base de datos
use tienda_online

// Verificar la base de datos actual
db
```

### 2. Crear Colecciones

```javascript
// Crear colecciones
db.createCollection("productos")
db.createCollection("clientes")

// Verificar las colecciones creadas
show collections
```

### 3. Insertar Documentos (Create)

```javascript
// Insertar un producto
db.productos.insertOne({
  nombre: "Laptop Pro",
  precio: 1299.99,
  categoria: "Electrónica",
  stock: 10,
  especificaciones: {
    procesador: "Intel i7",
    memoria: "16GB",
    almacenamiento: "512GB SSD"
  },
  colores_disponibles: ["Gris", "Plata"]
})

// Insertar varios productos a la vez
db.productos.insertMany([
  {
    nombre: "Smartphone X",
    precio: 699.99,
    categoria: "Electrónica",
    stock: 15,
    especificaciones: {
      procesador: "Octa-core",
      memoria: "8GB",
      almacenamiento: "128GB"
    },
    colores_disponibles: ["Negro", "Azul", "Rojo"]
  },
  {
    nombre: "Auriculares Inalámbricos",
    precio: 99.99,
    categoria: "Accesorios",
    stock: 30,
    especificaciones: {
      tipo: "In-ear",
      bateria: "24h",
      conexion: "Bluetooth 5.0"
    },
    colores_disponibles: ["Blanco", "Negro"]
  }
])

// Insertar clientes
db.clientes.insertMany([
  {
    nombre: "Ana López",
    email: "ana.lopez@ejemplo.com",
    direccion: {
      calle: "Calle Principal 123",
      ciudad: "Madrid",
      codigo_postal: "28001"
    },
    telefono: "+34600123456",
    fecha_registro: new Date("2023-01-15")
  },
  {
    nombre: "Carlos Martínez",
    email: "carlos.martinez@ejemplo.com",
    direccion: {
      calle: "Avenida Central 45",
      ciudad: "Barcelona",
      codigo_postal: "08001"
    },
    telefono: "+34600789012",
    fecha_registro: new Date("2023-02-20")
  }
])
```

### 4. Consultar Documentos (Read)

```javascript
// Mostrar todos los productos
db.productos.find()

// Mostrar todos los productos con formato legible
db.productos.find().pretty()

// Buscar productos por categoría
db.productos.find({ categoria: "Electrónica" })

// Buscar productos con precio mayor a 500€
db.productos.find({ precio: { $gt: 500 } })

// Mostrar solo ciertos campos (proyección)
db.productos.find({}, { nombre: 1, precio: 1, _id: 0 })

// Ordenar productos por precio (descendente)
db.productos.find().sort({ precio: -1 })

// Limitar resultados
db.productos.find().limit(2)

// Buscar documentos con condiciones específicas
db.productos.find({ 
  categoria: "Electrónica", 
  stock: { $gte: 10 } 
})

// Buscar un cliente específico
db.clientes.findOne({ nombre: "Ana López" })

// Contar documentos
db.productos.countDocuments({ categoria: "Electrónica" })
```

### 5. Actualizar Documentos (Update)

```javascript
// Actualizar un producto
db.productos.updateOne(
  { nombre: "Laptop Pro" },
  { $set: { precio: 1199.99, stock: 8 } }
)

// Actualizar múltiples productos
db.productos.updateMany(
  { categoria: "Electrónica" },
  { $set: { en_oferta: true } }
)

// Incrementar el stock
db.productos.updateOne(
  { nombre: "Auriculares Inalámbricos" },
  { $inc: { stock: 5 } }
)

// Añadir un elemento a un array
db.productos.updateOne(
  { nombre: "Laptop Pro" },
  { $push: { colores_disponibles: "Negro" } }
)

// Actualizar un documento anidado
db.clientes.updateOne(
  { nombre: "Carlos Martínez" },
  { $set: { "direccion.codigo_postal": "08005" } }
)
```

### 6. Eliminar Documentos (Delete)

```javascript
// Crear un producto temporal para luego eliminarlo
db.productos.insertOne({
  nombre: "Producto Temporal",
  precio: 9.99,
  categoria: "Otros"
})

// Eliminar un documento
db.productos.deleteOne({ nombre: "Producto Temporal" })

// Eliminar múltiples documentos
db.productos.deleteMany({ stock: { $lt: 5 } })

// Verificar que se eliminó correctamente
db.productos.find({ nombre: "Producto Temporal" })
```

### 7. Consultas con Operadores Lógicos

```javascript
// Operador OR
db.productos.find({ 
  $or: [
    { categoria: "Electrónica" },
    { precio: { $lt: 100 } }
  ]
})

// Operador AND
db.productos.find({
  $and: [
    { precio: { $gt: 500 } },
    { stock: { $gte: 10 } }
  ]
})

// Operador IN
db.productos.find({
  categoria: { $in: ["Electrónica", "Accesorios"] }
})
```

### 8. Explorar con MongoDB Compass

1. Abre MongoDB Compass y conéctate a tu instancia local
2. Navega a la base de datos `tienda_online`
3. Explora las colecciones `productos` y `clientes`
4. Usa el constructor de consultas de Compass para crear y ejecutar consultas
5. Edita un documento manualmente a través de la interfaz
6. Explora las estadísticas y la estructura de las colecciones

## Entregable

- Un archivo de texto con todos los comandos ejecutados y sus resultados
- Capturas de pantalla de MongoDB Compass mostrando:
  - La estructura de documentos en la colección `productos`
  - El resultado de al menos una consulta
  - La interfaz de edición de documentos

# Parte 3: Pequeño Proyecto Práctico

## Objetivo
Crear una pequeña aplicación que interactúe con MongoDB utilizando uno de los drivers oficiales.

## Instrucciones

Elige UNO de los siguientes ejercicios según tu lenguaje de programación preferido:

### Opción A: Node.js

1. Crea un nuevo proyecto Node.js:
```bash
mkdir tienda-mongodb
cd tienda-mongodb
npm init -y
npm install mongodb express
```

2. Crea un archivo `app.js` con el siguiente contenido:

```javascript
const express = require('express');
const { MongoClient } = require('mongodb');

const app = express();
app.use(express.json());

// URI de conexión a MongoDB
const uri = "mongodb://localhost:27017";
const client = new MongoClient(uri);

// Conexión a la base de datos
let db;

async function connectDB() {
  try {
    await client.connect();
    db = client.db("tienda_online");
    console.log("Conectado a MongoDB");
  } catch (error) {
    console.error("Error al conectar a MongoDB:", error);
    process.exit(1);
  }
}

// Rutas de la API
app.get('/productos', async (req, res) => {
  try {
    const productos = await db.collection("productos").find().toArray();
    res.json(productos);
  } catch (error) {
    res.status(500).json({ error: error.message });
  }
});

app.get('/productos/:nombre', async (req, res) => {
  try {
    const producto = await db.collection("productos").findOne({ 
      nombre: req.params.nombre 
    });
    
    if (!producto) {
      return res.status(404).json({ error: "Producto no encontrado" });
    }
    
    res.json(producto);
  } catch (error) {
    res.status(500).json({ error: error.message });
  }
});

app.post('/productos', async (req, res) => {
  try {
    const result = await db.collection("productos").insertOne(req.body);
    res.status(201).json({ 
      id: result.insertedId,
      ...req.body 
    });
  } catch (error) {
    res.status(500).json({ error: error.message });
  }
});

// Iniciar servidor
const PORT = 3000;
app.listen(PORT, async () => {
  await connectDB();
  console.log(`Servidor ejecutándose en http://localhost:${PORT}`);
});
```

3. Inicia la aplicación:
```bash
node app.js
```

4. Prueba la API con herramientas como Postman, curl o un navegador web.

### Opción B: Python

1. Crea un nuevo proyecto Python:
```bash
mkdir tienda-mongodb
cd tienda-mongodb
pip install pymongo flask
```

2. Crea un archivo `app.py` con el siguiente contenido:

```python
from flask import Flask, request, jsonify
from pymongo import MongoClient
from bson.json_util import dumps
import json

app = Flask(__name__)

# Conexión a MongoDB
client = MongoClient('mongodb://localhost:27017/')
db = client['tienda_online']

# Función para convertir BSON a JSON
def parse_json(data):
    return json.loads(dumps(data))

@app.route('/productos', methods=['GET'])
def get_productos():
    try:
        productos = list(db.productos.find())
        return jsonify(parse_json(productos))
    except Exception as e:
        return jsonify({"error": str(e)}), 500

@app.route('/productos/<nombre>', methods=['GET'])
def get_producto(nombre):
    try:
        producto = db.productos.find_one({"nombre": nombre})
        if producto:
            return jsonify(parse_json(producto))
        return jsonify({"error": "Producto no encontrado"}), 404
    except Exception as e:
        return jsonify({"error": str(e)}), 500

@app.route('/productos', methods=['POST'])
def add_producto():
    try:
        producto = request.json
        resultado = db.productos.insert_one(producto)
        nuevo_producto = db.productos.find_one({"_id": resultado.inserted_id})
        return jsonify(parse_json(nuevo_producto)), 201
    except Exception as e:
        return jsonify({"error": str(e)}), 500

if __name__ == '__main__':
    app.run(debug=True, port=3000)
```

3. Inicia la aplicación:
```bash
python app.py
```

4. Prueba la API con herramientas como Postman, curl o un navegador web.

## Entregable

- Código fuente de la aplicación
- Capturas de pantalla que muestren:
  - La aplicación en ejecución
  - Las pruebas realizadas a la API
  - Resultados de al menos una consulta GET y una POST
         codigo_postal: "28005"
       },
       telefono: "+34622334455",
       fecha_registro: new Date("2023-03-10")
     }
   ])
   ```

2. **Operaciones de agregación**:

   a. **Agrupar y contar**:
   ```javascript
   // Contar pedidos por estado
   db.pedidos.aggregate([
     { $group: { _id: "$estado", total: { $sum: 1 } } }
   ])
   
   // Contar productos por categoría
   db.productos.aggregate([
     { $group: { _id: "$categoria", cantidad: { $sum: 1 } } },
     { $sort: { cantidad: -1 } }
   ])
   ```

   b. **Calcular estadísticas**:
   ```javascript
   // Estadísticas de precios por categoría
   db.productos.aggregate([
     { $group: {
         _id: "$categoria",
         precio_promedio: { $avg: "$precio" },
         precio_minimo: { $min: "$precio" },
         precio_maximo: { $max: "$precio" },
         total_productos: { $sum: 1 }
       }
     },
     { $sort: { precio_promedio: -1 } }
   ])
   
   // Total de ventas por método de pago
   db.pedidos.aggregate([
     { $group: {
         _id: "$metodo_pago",
         total_ventas: { $sum: "$total" },
         cantidad_pedidos: { $sum: 1 }
       }
     },
     { $sort: { total_ventas: -1 } }
   ])
   ```

   c. **Lookups (Joins)**:
   ```javascript
   // Pedidos con información de cliente
   db.pedidos.aggregate([
     { $lookup: {
         from: "clientes",
         localField: "cliente_id",
         foreignField: "_id",
         as: "cliente_info"
       }
     },
     { $project: {
         fecha: 1,
         total: 1,
         estado: 1,
         "cliente_info.nombre": 1,
         "cliente_info.email": 1
       }
     }
   ])
   ```

   d. **Análisis de ventas por cliente**:
   ```javascript
   // Total de compras por cliente
   db.pedidos.aggregate([
     { $lookup: {
         from: "clientes",
         localField: "cliente_id",
         foreignField: "_id",
         as: "cliente_info"
       }
     },
     { $unwind: "$cliente_info" },
     { $group: {
         _id: "$cliente_info._id",
         nombre_cliente: { $first: "$cliente_info.nombre" },
         total_gastado: { $sum: "$total" },
         cantidad_pedidos: { $sum: 1 }
       }
     },
     { $sort: { total_gastado: -1 } }
   ])
   ```

   e. **Análisis de productos más vendidos**:
   ```javascript
   // Productos más vendidos
   db.pedidos.aggregate([
     { $unwind: "$productos" },
     { $group: {
         _id: "$productos.producto_id",
         unidades_vendidas: { $sum: "$productos.cantidad" },
         ingresos_totales: { $sum: { $multiply: ["$productos.cantidad", "$productos.precio_unitario"] } }
       }
     },
     { $lookup: {
         from: "productos",
         localField: "_id",
         foreignField: "_id",
         as: "producto_info"
       }
     },
     { $unwind: "$producto_info" },
     { $project: {
         nombre_producto: "$producto_info.nombre",
         categoria: "$producto_info.categoria",
         unidades_vendidas: 1,
         ingresos_totales: 1
       }
     },
     { $sort: { unidades_vendidas: -1 } }
   ])
   ```

   f. **Análisis de ventas por mes**:
   ```javascript
   // Ventas mensuales
   db.pedidos.aggregate([
     { $project: {
         year: { $year: "$fecha" },
         month: { $month: "$fecha" },
         total: 1
       }
     },
     { $group: {
         _id: { year: "$year", month: "$month" },
         ventas_totales: { $sum: "$total" },
         cantidad_pedidos: { $sum: 1 }
       }
     },
     { $sort: { "_id.year": 1, "_id.month": 1 } }
   ])
   ```

3. **Exportar resultados**:
   ```javascript
   // En la terminal (no en mongosh)
   // Exportar resultados de una agregación a un archivo JSON
   // Sustituye <pipeline> con la agregación deseada
   // mongoexport --db tienda_online --collection pedidos --query '<pipeline>' --out pedidos_analisis.json
   ```

### Entregable
- Archivo con todas las consultas de agregación y sus resultados
- Un informe (500 palabras) interpretando los resultados del análisis de datos
- Sugerencias para la empresa basadas en los resultados del análisis

## Ejercicio 5: Modelado de Datos Avanzado

### Objetivos
- Implementar diferentes patrones de diseño de datos
- Evaluar ventajas y desventajas de cada enfoque
- Optimizar el esquema para consultas específicas

### Instrucciones

1. **Implementar modelos con documentos embebidos**:
   ```javascript
   // Crear una nueva colección para productos con reseñas embebidas
   db.createCollection("productos_con_resenas")
   
   // Insertar productos con reseñas embebidas
   db.productos_con_resenas.insertMany([
     {
       nombre: "Laptop Pro",
       precio: 1199.99,
       categoria: "Electrónica",
       stock: 8,
       especificaciones: {
         procesador: "Intel i7",
         memoria: "16GB",
         almacenamiento: "512GB SSD"
       },
       resenas: [
         {
           usuario: "usuario1",
           puntuacion: 5,
           comentario: "Excelente laptop, muy rápida.",
           fecha: new Date("2023-02-10")
         },
         {
           usuario: "usuario2",
           puntuacion: 4,
           comentario: "Buena relación calidad-precio.",
           fecha: new Date("2023-03-15")
         }
       ]
     },
     {
       nombre: "Smartphone X",
       precio: 699.99,
       categoria: "Electrónica",
       stock: 20,
       especificaciones: {
         procesador: "Octa-core",
         memoria: "8GB",
         almacenamiento: "128GB"
       },
       resenas: [
         {
           usuario: "usuario3",
           puntuacion: 5,
           comentario: "Increíble cámara y rendimiento.",
           fecha: new Date("2023-03-01")
         }
       ]
     }
   ])
   
   // Consultar productos con sus reseñas
   db.productos_con_resenas.find()
   
   // Añadir una nueva reseña a un producto existente
   db.productos_con_resenas.updateOne(
     { nombre: "Laptop Pro" },
     { $push: { 
         resenas: {
           usuario: "usuario4",
           puntuacion: 3,
           comentario: "Buena laptop pero la batería dura poco.",
           fecha: new Date()
         }
       }
     }
   )
   ```

2. **Implementar modelos con referencias**:
   ```javascript
   // Crear colecciones separadas para productos y reseñas
   db.createCollection("productos_ref")
   db.createCollection("resenas")
   
   // Insertar productos
   db.productos_ref.insertMany([
     {
       _id: ObjectId(),
       nombre: "Laptop Pro",
       precio: 1199.99,
       categoria: "Electrónica",
       stock: 8,
       especificaciones: {
         procesador: "Intel i7",
         memoria: "16GB",
         almacenamiento: "512GB SSD"
       }
     },
     {
       _id: ObjectId(),
       nombre: "Smartphone X",
       precio: 699.99,
       categoria: "Electrónica",
       stock: 20,
       especificaciones: {
         procesador: "Octa-core",
         memoria: "8GB",
         almacenamiento: "128GB"
       }
     }
   ])
   
   // Insertar reseñas con referencias a productos
   db.resenas.insertMany([
     {
       producto_id: db.productos_ref.findOne({ nombre: "Laptop Pro" })._id,
       usuario: "usuario1",
       puntuacion: 5,
       comentario: "Excelente laptop, muy rápida.",
       fecha: new Date("2023-02-10")
     },
     {
       producto_id: db.productos_ref.findOne({ nombre: "Laptop Pro" })._id,
       usuario: "usuario2",
       puntuacion: 4,
       comentario: "Buena relación calidad-precio.",
       fecha: new Date("2023-03-15")
     },
     {
       producto_id: db.productos_ref.findOne({ nombre: "Smartphone X" })._id,
       usuario: "usuario3",
       puntuacion: 5,
       comentario: "Increíble cámara y rendimiento.",
       fecha: new Date("2023-03-01")
     }
   ])
   
   // Consultar un producto y sus reseñas (usando $lookup)
   db.productos_ref.aggregate([
     { $match: { nombre: "Laptop Pro" } },
     { $lookup: {
         from: "resenas",
         localField: "_id",
         foreignField: "producto_id",
         as: "resenas_producto"
       }
     }
   ])
   ```

3. **Implementar modelo híbrido**:
   ```javascript
   // Crear una colección para el modelo híbrido
   db.createCollection("productos_hibrido")
   
   // Insertar productos con información resumida de reseñas
   db.productos_hibrido.insertMany([
     {
       _id: ObjectId(),
       nombre: "Laptop Pro",
       precio: 1199.99,
       categoria: "Electrónica",
       stock: 8,
       especificaciones: {
         procesador: "Intel i7",
         memoria: "16GB",
         almacenamiento: "512GB SSD"
       },
       resumen_resenas: {
         total_resenas: 2,
         puntuacion_promedio: 4.5,
         ultima_actualizacion: new Date()
       }
     },
     {
       _id: ObjectId(),
       nombre: "Smartphone X",
       precio: 699.99,
       categoria: "Electrónica",
       stock: 20,
       especificaciones: {
         procesador: "Octa-core",
         memoria: "8GB",
         almacenamiento: "128GB"
       },
       resumen_resenas: {
         total_resenas: 1,
         puntuacion_promedio: 5.0,
         ultima_actualizacion: new Date()
       }
     }
   ])
   
   // Crear una colección para las reseñas detalladas
   db.createCollection("resenas_detalladas")
   
   // Insertar reseñas con referencias a productos
   db.resenas_detalladas.insertMany([
     {
       producto_id: db.productos_hibrido.findOne({ nombre: "Laptop Pro" })._id,
       usuario: "usuario1",
       puntuacion: 5,
       comentario: "Excelente laptop, muy rápida.",
       fecha: new Date("2023-02-10")
     },
     {
       producto_id: db.productos_hibrido.findOne({ nombre: "Laptop Pro" })._id,
       usuario: "usuario2",
       puntuacion: 4,
       comentario: "Buena relación calidad-precio.",
       fecha: new Date("2023-03-15")
     },
     {
       producto_id: db.productos_hibrido.findOne({ nombre: "Smartphone X" })._id,
       usuario: "usuario3",
       puntuacion: 5,
       comentario: "Increíble cámara y rendimiento.",
       fecha: new Date("2023-03-01")
     }
   ])
   
   // Consultar productos con su resumen de reseñas
   db.productos_hibrido.find()
   
   // Consultar un producto específico con todas sus reseñas detalladas
   db.productos_hibrido.aggregate([
     { $match: { nombre: "Laptop Pro" } },
     { $lookup: {
         from: "resenas_detalladas",
         localField: "_id",
         foreignField: "producto_id",
         as: "resenas_completas"
       }
     }
   ])
   ```

4. **Comparar rendimiento**:
   ```javascript
   // Medir rendimiento del modelo embebido
   db.productos_con_resenas.find({ nombre: "Laptop Pro" }).explain("executionStats")
   
   // Medir rendimiento del modelo con referencias
   db.productos_ref.aggregate([
     { $match: { nombre: "Laptop Pro" } },
     { $lookup: {
         from: "resenas",
         localField: "_id",
         foreignField: "producto_id",
         as: "resenas_producto"
       }
     }
   ]).explain("executionStats")
   
   // Medir rendimiento del modelo híbrido
   db.productos_hibrido.find({ nombre: "Laptop Pro" }).explain("executionStats")
   db.