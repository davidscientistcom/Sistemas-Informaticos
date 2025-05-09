# Práctica: Generador de Contratos de Agua con Sensores y Almacenamiento en MongoDB

## Objetivo

Diseñar y ejecutar un sistema que genere automáticamente contratos de agua simulados y los almacene en una base de datos MongoDB alojada en un contenedor Docker. Cada contrato debe incluir información del cliente, su consumo y los datos técnicos de un contador inteligente asignado aleatoriamente.

## Competencias que se trabajan

* Uso de bases de datos NoSQL (MongoDB).
* Gestión de contenedores con Docker.
* Programación en Python con generación de datos falsos (Faker).
* Inserción y consulta de datos con PyMongo.
* Modelado de estructuras jerárquicas y semiestructuradas en MongoDB.

## Requisitos previos

* Tener Docker y Python 3 instalados.
* Conocimientos básicos de MongoDB y JSON.
* Familiaridad con Python y estructuras de datos.

## Enunciado de la práctica

Se desea simular el alta de contratos de agua en una ciudad inteligente. Cada contrato tendrá los siguientes campos:

* `id_contrato`: número único de contrato.
* `nombre_cliente`: nombre de la persona titular.
* `direccion`: dirección simulada del suministro.
* `fecha_alta`: fecha de activación del contrato.
* `tarifa`: tipo de tarifa asignada (residencial, comercial, industrial).
* `consumo_estimado`: consumo medio estimado del cliente.
* `contador`: objeto embebido con la información técnica del contador asignado.

El campo `contador` deberá tener la siguiente estructura:

```json
"contador": {
    "id_contador": "C1234567",
    "modelo": "AquaFlow X300",
    "tipo": "gran_consumidor", 
    "coordenadas": {
        "latitud": 38.3452,
        "longitud": -0.4810
    },
    "ano_fabricacion": 2021
}
```

### Requisitos obligatorios del script

1. Utilizar la librería `Faker` para generar datos de clientes de forma aleatoria.
2. Diseñar una función que genere un objeto `contador` aleatorio, incluyendo:

   * Identificador único (al estilo `Cxxxxxxx`).
   * Modelo aleatorio a partir de un conjunto cerrado de nombres (por ejemplo: `["AquaFlow X300", "HydroSmart Z2", "EcoMeter T9"]`).
   * Tipo de contador: puede ser `domestico`, `gran_consumidor`, o `industrial`.
   * Coordenadas aleatorias dentro de un rango razonable de latitud y longitud (por ejemplo, correspondientes a una ciudad).
   * Año de fabricación entre 2015 y 2024.
3. Generar al menos 50 contratos distintos.
4. Insertar todos los contratos generados en MongoDB utilizando PyMongo.
5. Verificar desde la shell de MongoDB o MongoDB Compass que los datos han sido insertados correctamente.

## Indicaciones

* Utilizar una estructura jerárquica en los documentos, embebiendo el objeto `contador` dentro del contrato.
* El contenedor de MongoDB debe estar corriendo en el puerto 27017.
* Documentar brevemente el código con comentarios.
* Usar nombres de funciones claros y significativos.
* Los datos generados deben guardarse además en un archivo `contratos_generados.json`.

## Preguntas de reflexión (para entregar junto al código)

1. ¿Por qué tiene sentido usar una base de datos NoSQL como MongoDB para este tipo de datos?
2. ¿Qué ventajas ofrece el uso de estructuras embebidas (documentos anidados) frente a una base de datos relacional?
3. Si tuvieras que escalar este sistema a miles de contratos por segundo, ¿qué aspectos técnicos deberías mejorar?
