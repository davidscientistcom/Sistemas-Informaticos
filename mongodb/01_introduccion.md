# MongoDB: Teoría, Instalación, Generación de Datos y CRUD

## 1. Introducción a NoSQL y MongoDB

### 1.1 ¿Qué es una base de datos NoSQL?

NoSQL significa "Not Only SQL" y se refiere a bases de datos no relacionales que almacenan los datos en formatos como documentos, clave-valor, grafos o columnas. Se caracterizan por:

- Modelo de esquema flexible (no requieren esquemas fijos).
- Escalabilidad horizontal (distribuir en muchos servidores).
- Soporte para grandes volúmenes de datos.
- Rendimiento optimizado para ciertos patrones (lectura, escritura, consultas simples).
- Consistencia eventual en lugar de estricta consistencia ACID.

#### Tipos principales de bases de datos NoSQL:

- **Documentos** (ej. MongoDB): datos estructurados como JSON/BSON.
- **Clave-valor** (ej. Redis): pares clave → valor.
- **Columnas** (ej. Cassandra): orientadas a columnas.
- **Grafos** (ej. Neo4j): estructuras con nodos y relaciones.

### 1.2 Diferencias entre SQL y NoSQL

| Característica              | Relacional (SQL)                  | NoSQL                              |
||--|-|
| Estructura de datos          | Tablas (filas y columnas)         | Documentos, clave-valor, grafos     |
| Esquema                      | Fijo y predefinido                | Flexible o sin esquema              |
| Escalabilidad                | Vertical                          | Horizontal                          |
| Consistencia                 | ACID                              | BASE (eventualmente consistente)    |
| Lenguaje de consulta         | SQL                               | APIs, JSON, operadores propios      |
| Ejemplos                     | MySQL, PostgreSQL, Oracle         | MongoDB, Redis, Cassandra, Neo4j    |

### 1.3 ¿Qué es MongoDB?

MongoDB es una base de datos orientada a documentos (JSON extendido: BSON). Los conceptos clave son:

- **Base de datos:** contenedor general de datos.
- **Colección:** equivalente a tabla, agrupa documentos.
- **Documento:** objeto BSON que representa un registro.
- **_id:** clave primaria única de cada documento.

### 1.4 Beneficios de MongoDB

- Esquema flexible.
- Escalabilidad horizontal.
- Buen rendimiento en consultas.
- Compatible con estructuras de datos en lenguajes populares.
- Ideal para cloud y entornos distribuidos.



## 2. Instalación de MongoDB con Docker

### 2.1 Requisitos previos

Instalar [Docker](https://www.docker.com/) en tu sistema operativo.

```bash
# Comprobar versión
docker --version
```

### 2.2 Descargar imagen oficial

```bash
docker pull mongodb/mongodb-community-server:latest
```

Para una versión específica:

```bash
docker pull mongodb/mongodb-community-server:5.0
```

### 2.3 Ejecutar contenedor

```bash
docker run --name mongodb -p 27017:27017 -d mongodb/mongodb-community-server:latest
```

- `--name`: nombre del contenedor
- `-p`: mapea puerto 27017 al host
- `-d`: modo en segundo plano

### 2.4 Verificar estado

```bash
docker container ls
docker exec -it mongodb mongosh
```

```js
db.runCommand({ hello: 1 })
```



## 3. Generación de Datos Realistas con Python Faker

### 3.1 Instalar Faker

```bash
pip install Faker
```

### 3.2 Modelo de datos de contrato de agua

- id_contrato
- nombre_cliente
- direccion
- fecha_alta
- tarifa
- lecturas_contador
- consumo

### 3.3 Script Python para generar datos

```python
from faker import Faker
import json
from datetime import datetime
import random

fake = Faker('es_ES')

def generar_lectura_contador():
    return {
        'fecha': fake.date_time_this_year().isoformat(),
        'valor': fake.random_number(digits=5)
    }

def generar_contrato():
    tipo_tarifa = random.choice(['residencial', 'comercial', 'industrial'])
    fecha_alta = fake.date_between(start_date='-5y', end_date='today').isoformat()
    lecturas = [generar_lectura_contador() for _ in range(random.randint(3, 10))]
    consumo = sum(lectura['valor'] for lectura in lecturas) / len(lecturas)
    return {
        'id_contrato': fake.unique.random_number(digits=8),
        'nombre_cliente': fake.name(),
        'direccion': fake.address(),
        'fecha_alta': fecha_alta,
        'tarifa': tipo_tarifa,
        'lecturas_contador': lecturas,
        'consumo': round(consumo, 2)
    }

if __name__ == "__main__":
    contratos = [generar_contrato() for _ in range(10)]
    with open("contratos_agua.json", "w") as f:
        json.dump(contratos, f, indent=4, ensure_ascii=False)
```



## 4. Operaciones CRUD con PyMongo

### 4.1 Instalación

```bash
pip install pymongo
```

### 4.2 Conexión a MongoDB

```python
from pymongo import MongoClient
client = MongoClient("localhost", 27017)
db = client["water_contracts"]
contracts = db["contracts"]
```

### 4.3 Crear documentos

```python
# Un solo documento
contracts.insert_one({ 'id_contrato': 1, 'nombre_cliente': 'Juan', ... })

# Múltiples documentos
with open('contratos_agua.json') as f:
    data = json.load(f)
contracts.insert_many(data)
```

### 4.4 Leer documentos

```python
# Todos
for doc in contracts.find():
    print(doc)

# Filtro
contracts.find_one({'id_contrato': 12345678})
contracts.find({ 'consumo': { '$gt': 100 } })
```

### 4.5 Actualizar documentos

```python
contracts.update_one(
    { 'id_contrato': 12345678 },
    { '$set': { 'consumo': 200 } }
)

contracts.update_many(
    { 'tarifa': 'residencial' },
    { '$inc': { 'consumo': 5 } }
)
```

### 4.6 Eliminar documentos

```python
contracts.delete_one({ 'id_contrato': 12345678 })
contracts.delete_many({ 'consumo': 0 })
```



## 5. Uso de MongoDB Compass

### 5.1 ¿Qué es MongoDB Compass?

GUI oficial de MongoDB para explorar y gestionar bases de datos visualmente. Permite:

- Ver documentos y esquemas
- Ejecutar queries
- Visualizar rendimiento
- Crear índices

### 5.2 Conexión a Compass

- Host: `localhost`
- Puerto: `27017`

### 5.3 Acciones comunes

- Navegar por bases de datos y colecciones
- Ver documentos en tabla o JSON
- Usar el constructor visual de queries
- Editar o eliminar registros



## 6. Conclusión

MongoDB es una base de datos NoSQL potente, flexible y moderna. Este documento te ha guiado desde la teoría hasta la práctica completa:

- Fundamentos NoSQL y diferencias con SQL
- Instalación con Docker
- Generación de datos realistas
- CRUD con PyMongo y shell
- Visualización con MongoDB Compass

Es ideal para proyectos que requieren agilidad, escalabilidad y estructuras de datos diversas. Para seguir aprendiendo, se recomienda profundizar en:

- Agregaciones
- Modelado de datos
- Indexación
- Seguridad y despliegue en producción

Consulta siempre la [documentación oficial de MongoDB](https://www.mongodb.com/docs/) para estar al día con las mejores prácticas.

