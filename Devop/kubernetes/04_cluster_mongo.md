# Cluster de MongoDB: Fundamentos, Arquitectura y Operación

##  Objetivo

Este documento explica **cómo funciona un clúster de MongoDB**, qué componentes tiene, cómo se configura y qué aspectos hay que tener en cuenta para garantizar la alta disponibilidad y consistencia de los datos.

No trabajaremos aún con Kubernetes. Este artefacto sirve para entender perfectamente **qué requisitos debe cumplir MongoDB para funcionar en modo clúster**, y cómo responder ante fallos, elecciones de nodo primario, y replicación de datos.



##  ¿Qué es un clúster de MongoDB?

Un clúster de MongoDB es una **infraestructura distribuida** que permite:

* Alta disponibilidad
* Escalado horizontal o vertical
* Tolerancia a fallos
* Reparto o replicación de los datos

MongoDB tiene dos mecanismos principales:

1. **Replica Set**: Mismo conjunto de datos replicado entre nodos
2. **Sharding**: Reparto de datos entre varios shards (para volumen masivo)

Nos centraremos en **Replica Set**, que es la base para cualquier sistema tolerante a fallos, pero también introduciremos **sharding** como modelo de escalado horizontal.



##  1. Arquitectura de un Replica Set

Un Replica Set está formado por:

* 🔑 **Primary**: nodo que recibe escrituras y lecturas (por defecto)
* 🔍 **Secondary**: replica el contenido del primary; puede aceptar lecturas si se configura
* 🚫 **Arbiter** (opcional): no almacena datos, pero ayuda a desempatar elecciones

Se recomienda tener un número impar de nodos (3, 5, 7...) para garantizar **quórum**.



##  2. Mecanismo de Elección y Quórum

### ¿Qué es el quórum?

En sistemas distribuidos como MongoDB, el quórum es el número mínimo de nodos que deben estar de acuerdo para tomar decisiones, como **elegir un nuevo nodo primario**. MongoDB usa un protocolo de consenso basado en votación.

* Cada nodo tiene un voto (máximo 7 votos por Replica Set)
* Para elegir un nuevo primary, se necesita más de la mitad de los votos disponibles

**Ejemplo:**

* 3 nodos → quórum mínimo: 2
* 5 nodos → quórum mínimo: 3

### ¿Qué pasa si el nodo primary cae?

1. Los nodos restantes detectan la pérdida mediante heartbeats (cada 2 segundos por defecto)
2. Inician una **elección** (election)
3. Gana el nodo con:

   * Datos más actualizados (mayor `optime`)
   * Mayor prioridad (si se ha configurado)

Si no hay quórum:

* El clúster entra en **modo de solo lectura** (no hay primary)
* Se evita la inconsistencia de datos al evitar escrituras simultáneas



##  3. Sharding: Escalado Horizontal

### ¿Qué es sharding?

Sharding es la técnica de **dividir los datos entre múltiples clústeres o nodos** para permitir manejar volúmenes masivos de datos y distribuir la carga.

Un clúster shardeado en MongoDB está compuesto por:

* **Shards**: Replica sets que contienen fragmentos de los datos
* **Config Servers**: Guardan metadatos y mapeo de qué shard tiene cada dato
* **Mongos**: Router que recibe las peticiones del cliente y las dirige al shard adecuado

### ¿Cómo se decide dónde va cada dato?

Los datos se dividen en **rangos o hashes** según una **clave shard**. Por ejemplo, si la clave es `cliente_id`, Mongo puede guardar los IDs 1–1000 en el shard A, 1001–2000 en el shard B, etc.

### ¿Y si un shard se cae?

Como cada shard es un Replica Set, **mantiene alta disponibilidad** por sí mismo. Sin embargo, si el Config Server falla, se puede perder la capacidad de direccionar correctamente las consultas.



##  4. Configuración inicial de un Replica Set (modo manual)

Supongamos que tenemos 3 servidores MongoDB: `mongo1`, `mongo2`, `mongo3`

### Paso 1: Lanzar instancias con el mismo `--replSet`:

```bash
mongod --replSet rs0 --port 27017 --bind_ip localhost,mongo1
mongod --replSet rs0 --port 27017 --bind_ip localhost,mongo2
mongod --replSet rs0 --port 27017 --bind_ip localhost,mongo3
```

### Paso 2: Conectarse a uno y configurar el Replica Set

```bash
mongo --host mongo1
```

```js
rs.initiate({
  _id: "rs0",
  members: [
    { _id: 0, host: "mongo1:27017" },
    { _id: 1, host: "mongo2:27017" },
    { _id: 2, host: "mongo3:27017" }
  ]
})
```

### Paso 3: Ver el estado

```js
rs.status()
```



##  5. Preguntas comunes de examen o entrevista

###  ¿Cuántos nodos necesito para alta disponibilidad?

Mínimo **3 nodos** (2 data-bearing + 1 arbiter o 3 full replicas)

###  ¿Puedo tener dos primaries?

No. Solo puede haber **un primary a la vez**. Si hay partición de red, los nodos sin quórum no eligen nuevo primary.

###  ¿Y si uso un arbiter?

El arbiter permite quórum en números pares, pero **no almacena datos** y **no debe usarse si ya hay 3 nodos data-bearing**.

###  ¿Qué es la prioridad en los nodos?

Puedes asignar una `priority` para controlar qué nodo tiene más probabilidades de ser elegido como primary:

```js
{ _id: 0, host: "mongo1:27017", priority: 2 }
```

###  ¿Qué puerto usa MongoDB?

Por defecto: `27017`. Cada instancia debe tener este puerto libre.



##  6. Mantenimiento y operación

### Ver el estado en tiempo real:

```bash
mongo --host mongo1
rs.status()
```

### Forzar nueva elección:

```js
rs.stepDown()
```

### Añadir un nuevo nodo:

```js
rs.add("mongo4:27017")
```

### Eliminar un nodo:

```js
rs.remove("mongo2:27017")
```



##  7. Consideraciones importantes

* Los relojes del sistema deben estar **sincronizados** (usar `ntpd` o `chronyd`)
* Los nodos deben tener **visibilidad entre ellos** (resolución DNS o `/etc/hosts`)
* No exponer directamente los nodos a internet sin control de accesos y TLS
* Configurar correctamente las **read/write concerns** para ajustar consistencia y disponibilidad


##  8. Conclusión

Un clúster de MongoDB basado en Replica Set proporciona una arquitectura sencilla y potente para alta disponibilidad. Sharding permite escalar horizontalmente y soportar peticiones masivas.

Entender estos principios es fundamental para desplegar MongoDB correctamente en Kubernetes, donde cada pod actuará como uno de estos nodos.
