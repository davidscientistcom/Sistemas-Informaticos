# MongoDB en Kubernetes — Monitorización con Prometheus y Grafana

##  Objetivo

En este documento aprenderemos a monitorizar MongoDB dentro de un clúster de Kubernetes usando:

* **Prometheus**: recolector de métricas
* **MongoDB Exporter**: puente entre MongoDB y Prometheus
* **Grafana**: visualización de datos en tiempo real



##  1. Arquitectura de la monitorización

Para entender cómo se integran MongoDB, Prometheus y Grafana, veamos el flujo de datos:

```
MongoDB ─▶ MongoDB Exporter ─▶ Prometheus ─▶ Grafana
```

* **MongoDB**: nuestra base de datos desplegada previamente como `StatefulSet`
* **MongoDB Exporter**: un contenedor que expone métricas de MongoDB en formato Prometheus
* **Prometheus**: herramienta que consulta periódicamente al Exporter y guarda esas métricas
* **Grafana**: herramienta web que accede a Prometheus y muestra las métricas en paneles gráficos



## 🔌 2. MongoDB Exporter

### ¿Qué es un Exporter?

Un Exporter es un servicio auxiliar que **traduce los datos internos de un sistema (como MongoDB)** a un formato comprensible por Prometheus. En este caso, usaremos `percona/mongodb_exporter`, una imagen que ya viene preparada para MongoDB 4.x y 5.x.

### Paso 1: Crear un usuario de solo lectura para el Exporter

El Exporter necesita conectarse a MongoDB para extraer métricas. Por seguridad, **no debe usar el usuario `admin`**, sino un usuario con rol `clusterMonitor`:

```bash
kubectl exec -it mongo-0 -- mongo -u admin -p secreto123 --authenticationDatabase admin
```

```js
db.getSiblingDB("admin").createUser({
  user: "metrics",
  pwd: "metrics123",
  roles: [ { role: "clusterMonitor", db: "admin" } ]
})
```

### Paso 2: Desplegar el Exporter

Creamos un `Deployment` con un solo pod:

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: mongodb-exporter
spec:
  replicas: 1
  selector:
    matchLabels:
      app: mongodb-exporter
  template:
    metadata:
      labels:
        app: mongodb-exporter
    spec:
      containers:
      - name: exporter
        image: percona/mongodb_exporter:0.40.0
        ports:
        - containerPort: 9216
        env:
        - name: MONGODB_URI
          value: mongodb://metrics:metrics123@mongo-0.mongo:27017/admin?replicaSet=rs0
```

### Paso 3: Crear el `Service`

```yaml
apiVersion: v1
kind: Service
metadata:
  name: mongodb-exporter
spec:
  selector:
    app: mongodb-exporter
  ports:
    - name: http
      port: 9216
      targetPort: 9216
```

> Al aplicar ambos recursos con `kubectl apply`, se despliega el Exporter y queda accesible en el clúster. Puedes verificar con:

```bash
kubectl get svc mongodb-exporter
```



##  3. ¿Qué es Helm y por qué lo usamos?

**Helm** es un gestor de paquetes para Kubernetes, similar a `apt` o `yum`, pero para clústeres. Permite instalar aplicaciones complejas (como Prometheus o Grafana) con una sola línea, en lugar de definir múltiples YAML manualmente.

**Ventajas de Helm:**

* Instala configuraciones completas con buenas prácticas por defecto
* Permite personalizar variables con `--set`
* Mantiene las versiones organizadas y listas para actualizar/desinstalar



## 📊 4. Instalar Prometheus con Helm

### Paso 1: Instalar Helm (si no lo tienes)

```bash
curl https://raw.githubusercontent.com/helm/helm/master/scripts/get-helm-3 | bash
```

### Paso 2: Añadir el repositorio oficial

```bash
helm repo add prometheus-community https://prometheus-community.github.io/helm-charts
helm repo update
```

### Paso 3: Instalar Prometheus

```bash
helm install prometheus prometheus-community/prometheus \
  --set server.service.type=NodePort
```

Este comando despliega:

* Prometheus server (interfaz y recolector de métricas)
* Alertmanager (lo veremos en otro artefacto)
* Exporters opcionales (CPU, red, etc.)

Para abrir la interfaz web:

```bash
minikube service prometheus-server
```

## ⚙️ 5. Configurar Prometheus para scrapear el Exporter

Prometheus necesita saber qué endpoints debe consultar. Esto se configura en su archivo `prometheus.yml`, que podemos modificar a través de un `ConfigMap`:

### Paso 1: Localiza el ConfigMap

```bash
kubectl get configmap -l app=prometheus
```

### Paso 2: Edita el archivo

```bash
kubectl edit configmap prometheus-server
```

Busca la sección `scrape_configs:` y añade:

```yaml
  - job_name: 'mongodb'
    static_configs:
      - targets: ['mongodb-exporter.default.svc.cluster.local:9216']
```

### Paso 3: Reinicia Prometheus para que lea los cambios

```bash
kubectl delete pod -l app=prometheus,component=server
```



##  6. Instalar y configurar Grafana

### Paso 1: Instalar Grafana

```bash
helm repo add grafana https://grafana.github.io/helm-charts
helm repo update
helm install grafana grafana/grafana \
  --set service.type=NodePort \
  --set adminPassword=grafana123
```

### Paso 2: Abrir la interfaz web

```bash
minikube service grafana
```

Credenciales:

* Usuario: `admin`
* Contraseña: `grafana123`

### Paso 3: Conectar Grafana con Prometheus

1. Ir a `Configuration > Data Sources`
2. Elegir `Add data source`
3. Tipo: `Prometheus`
4. URL: `http://prometheus-server.default.svc.cluster.local`
5. Guardar

### Paso 4: Importar dashboard oficial de MongoDB

1. Ir a `Dashboards > Import`
2. Usar ID: `2583` (MongoDB Overview)
3. Asignar la fuente de datos Prometheus



##  7. Verificación final

* Ir a Prometheus > Status > Targets > Verificar que `mongodb-exporter` esté marcado como "UP"
* Ver en Grafana los paneles de:

  * Conexiones activas
  * Operaciones por segundo
  * Memoria usada



##  Conclusión

Has aprendido a montar un sistema profesional de monitorización para MongoDB, usando las herramientas más utilizadas en la industria:

* Exporter: convierte datos de Mongo a métricas
* Prometheus: recolecta y almacena métricas
* Grafana: visualiza esas métricas en tiempo real
* Helm: simplifica la instalación de componentes complejos
