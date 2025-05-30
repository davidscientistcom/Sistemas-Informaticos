# Despliegue de un Cluster MongoDB en Kubernetes — Paso a Paso con Explicaciones

##  Objetivo

Este documento tiene como finalidad desplegar un **Replica Set de MongoDB** (es decir, un clúster tolerante a fallos) dentro de Kubernetes utilizando **StatefulSets**, **volúmenes persistentes**, y **servicios headless**.

Vamos a:

1. Entender por qué usamos ciertos recursos de Kubernetes
2. Configurar 3 réplicas de MongoDB
3. Inicializar el Replica Set desde uno de los pods
4. Verificar que el clúster funciona



##  Requisitos previos

* Kubernetes activo (por ejemplo, en Minikube)
* `kubectl` configurado
* Conocimientos previos del funcionamiento de un Replica Set (ver artefacto anterior)



##  1. Creamos el almacenamiento persistente (PersistentVolumeClaims)

**¿Por qué es necesario?**
MongoDB guarda los datos dentro del contenedor, pero si el pod se reinicia o se programa en otro nodo, **todos los datos se perderían** si no usamos un volumen persistente.

Un `PersistentVolumeClaim` (PVC) permite solicitar a Kubernetes un almacenamiento duradero que **sobrevive a reinicios del pod**.

Archivo: `mongo-pvc.yaml`

```yaml
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: mongo-pvc
spec:
  accessModes:
    - ReadWriteOnce
  resources:
    requests:
      storage: 1Gi
```



##  2. Creamos el Headless Service

### ¿Qué es un servicio en Kubernetes?

Un `Service` en Kubernetes actúa como un punto de acceso estable para los pods, ya que **los pods pueden cambiar de IP** cuando se reinician. El Service permite que los clientes se conecten sin importar estos cambios.

### ¿Y un servicio headless?

Un **servicio headless** se define con `clusterIP: None`. Esto quiere decir:

* **No** se le asigna una IP virtual al servicio
* En su lugar, **cada pod tiene su nombre DNS propio** generado por Kubernetes
* Esto es crucial para `StatefulSet`, ya que permite que `mongo-0`, `mongo-1`, `mongo-2` se vean entre sí por nombre

> Si no usáramos un headless service, no podríamos construir correctamente un Replica Set con nombres predecibles.

Archivo: `mongo-headless-service.yaml`

```yaml
apiVersion: v1
kind: Service
metadata:
  name: mongo
spec:
  clusterIP: None
  selector:
    app: mongo
  ports:
    - name: mongo
      port: 27017
      targetPort: 27017
```

### Resultado del headless:

Una vez desplegado el StatefulSet, Kubernetes genera DNS como:

* `mongo-0.mongo.default.svc.cluster.local`
* `mongo-1.mongo.default.svc.cluster.local`
* `mongo-2.mongo.default.svc.cluster.local`

Estos nombres nos permiten **inicializar el Replica Set** sin tener que resolver IPs dinámicas manualmente.



## 🚀 3. Creamos el StatefulSet

Un `StatefulSet` es como un `Deployment` pero **con identidad persistente** para cada réplica. Es ideal para bases de datos porque:

* Cada pod mantiene su nombre (ej. `mongo-0`, `mongo-1`)
* Cada pod tiene un volumen exclusivo
* El orden de creación y eliminación de pods está controlado

Archivo: `mongo-statefulset.yaml`

```yaml
apiVersion: apps/v1
kind: StatefulSet
metadata:
  name: mongo
spec:
  serviceName: mongo
  replicas: 3
  selector:
    matchLabels:
      app: mongo
  template:
    metadata:
      labels:
        app: mongo
    spec:
      containers:
      - name: mongo
        image: mongo:6.0
        ports:
        - containerPort: 27017
        volumeMounts:
        - name: mongo-data
          mountPath: /data/db
        command: ["mongod"]
        args: ["--replSet", "rs0", "--bind_ip_all"]
  volumeClaimTemplates:
  - metadata:
      name: mongo-data
    spec:
      accessModes: ["ReadWriteOnce"]
      resources:
        requests:
          storage: 1Gi
```

**Explicaciones clave:**

* `volumeClaimTemplates`: Cada pod tendrá su propio volumen persistente
* `--replSet rs0`: todos los nodos se unen al mismo Replica Set
* `--bind_ip_all`: permite conexiones entrantes desde otros pods del clúster



##  4. Desplegamos los recursos

```bash
kubectl apply -f mongo-pvc.yaml
kubectl apply -f mongo-headless-service.yaml
kubectl apply -f mongo-statefulset.yaml
```

Verificamos que se crean los pods:

```bash
kubectl get pods -l app=mongo
kubectl get svc mongo
```



##  5. Inicializamos el Replica Set

Una vez todos los pods estén `Running`, entramos al primero:

```bash
kubectl exec -it mongo-0 -- mongo
```

Inicializamos:

```js
rs.initiate({
  _id: "rs0",
  members: [
    { _id: 0, host: "mongo-0.mongo:27017" },
    { _id: 1, host: "mongo-1.mongo:27017" },
    { _id: 2, host: "mongo-2.mongo:27017" }
  ]
})
```

Verificamos:

```js
rs.status()
```



## 🔍 6. Comprobación funcional

### a) Insertar datos en el primary

```bash
kubectl exec -it mongo-0 -- mongo
```

```js
db = db.getSiblingDB("test")
db.users.insert({nombre: "Lex", rol: "admin"})
```

### b) Consultar desde otro nodo

```bash
kubectl exec -it mongo-1 -- mongo
```

```js
rs.slaveOk()
db = db.getSiblingDB("test")
db.users.find()
```



##  7. Simular fallo de nodo primario

```bash
kubectl delete pod mongo-0
```

Kubernetes lo reiniciará, pero mientras tanto:

* Otro nodo debe ser elegido como nuevo `PRIMARY`
* Los datos siguen accesibles desde los demás nodos

Puedes comprobarlo con:

```bash
kubectl exec -it mongo-1 -- mongo
rs.status()
```



##  8. Conclusiones y siguiente paso

* Hemos replicado fielmente una topología de alta disponibilidad con MongoDB
* Hemos usado herramientas de Kubernetes que garantizan identidad de pod, DNS estable y almacenamiento persistente
* Hemos comprobado que se produce replicación real y failover
