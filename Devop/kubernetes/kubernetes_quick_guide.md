# Guía Práctica de Kubernetes - Conceptos Fundamentales y Implementación

## Introducción a Kubernetes

Kubernetes es una plataforma de orquestación de contenedores que automatiza el despliegue, escalado y gestión de aplicaciones contenedorizadas. Mientras que Docker se enfoca en la creación y ejecución de contenedores individuales, Kubernetes gestiona estos contenedores a escala en un entorno distribuido.

**Kubernetes proporciona:**
- Orquestación automática de contenedores
- Auto-recuperación ante fallos
- Escalado horizontal automático y manual
- Descubrimiento de servicios y balanceo de carga
- Gestión de actualizaciones sin downtime
- Gestión centralizada de configuración y secretos

## Arquitectura y Conceptos Fundamentales

### Cluster
Un cluster de Kubernetes es el conjunto completo de recursos computacionales (nodos) que ejecutan aplicaciones contenedorizadas. Consiste en:
- **Control Plane**: Gestiona el estado del cluster y toma decisiones
- **Worker Nodes**: Ejecutan las aplicaciones (Pods)

### Pod
El Pod es la unidad de despliegue más pequeña en Kubernetes. Características clave:
- Puede contener uno o más contenedores estrechamente acoplados
- Los contenedores del mismo Pod comparten red y almacenamiento
- Cada Pod tiene una dirección IP única dentro del cluster
- Los Pods son efímeros por naturaleza

### Deployment
Un Deployment define el estado deseado de un conjunto de Pods idénticos:
- Gestiona la creación y actualización de ReplicaSets
- Garantiza que el número deseado de réplicas esté ejecutándose
- Permite actualizaciones rolling y rollbacks
- Proporciona declaratividad en la gestión del ciclo de vida

### Service
Un Service abstrae el acceso a un conjunto de Pods:
- Proporciona una dirección IP y nombre DNS estables
- Balancea la carga entre los Pods backend
- Desacopla los consumidores de los Pods específicos
- Permite diferentes tipos de exposición (ClusterIP, NodePort, LoadBalancer)

### kubectl
kubectl es la interfaz de línea de comandos para interactuar con la API de Kubernetes. Permite gestionar todos los recursos del cluster de forma declarativa e imperativa.

## Configuración del Entorno

### Opción 1: Docker Desktop
1. Instalar Docker Desktop
2. Acceder a Settings → Kubernetes
3. Activar "Enable Kubernetes"
4. Aplicar y reiniciar

### Opción 2: Minikube
```bash
# Instalación en Windows con Chocolatey
choco install minikube

# Iniciar cluster local
minikube start --driver=virtualbox  # o --driver=hyperv

# Verificar instalación
kubectl cluster-info
kubectl get nodes
```

## Comandos Esenciales

### Información del Cluster
```bash
# Estado del cluster
kubectl cluster-info

# Nodos disponibles
kubectl get nodes

# Información detallada de un nodo
kubectl describe node <nombre-nodo>
```

### Gestión de Recursos
```bash
# Listar recursos por tipo
kubectl get pods
kubectl get services
kubectl get deployments

# Información detallada de un recurso
kubectl describe pod <nombre-pod>
kubectl describe service <nombre-service>

# Logs de aplicaciones
kubectl logs <nombre-pod>
kubectl logs -f <nombre-pod>  # seguimiento en tiempo real

# Aplicar configuraciones
kubectl apply -f <archivo.yaml>

# Eliminar recursos
kubectl delete -f <archivo.yaml>
kubectl delete pod <nombre-pod>
```

## Red y Asignación de IPs en Kubernetes

### Modelo de Red de Kubernetes

Kubernetes implementa un modelo de red plano donde:

**Asignación de IPs a Pods:**
- Cada Pod recibe una IP única del rango CIDR del cluster
- Esta IP es asignada por el plugin CNI (Container Network Interface)
- La IP del Pod es efímera y cambia cuando el Pod se reinicia
- Ejemplo: Pod puede tener IP 10.244.1.23 del rango 10.244.0.0/16

**Comunicación Interna:**
- Los Pods se pueden comunicar directamente usando sus IPs
- No hay NAT entre Pods del mismo cluster
- Los contenedores dentro del mismo Pod comparten localhost (127.0.0.1)

### Services y Asignación de Puertos

**ClusterIP (por defecto):**
- El Service recibe una IP virtual del rango de servicios (ej: 10.96.0.0/12)
- Esta IP es estable durante la vida del Service
- Ejemplo: Service obtiene IP 10.96.245.132

```yaml
spec:
  type: ClusterIP
  ports:
  - port: 80        # Puerto del Service (donde escucha)
    targetPort: 8080 # Puerto del contenedor en el Pod
```

**NodePort:**
- Asigna un puerto alto (30000-32767) en todos los nodos
- Kubernetes selecciona automáticamente o puedes especificar uno
- El tráfico se reenvía: Node:NodePort → Service:Port → Pod:TargetPort

```yaml
spec:
  type: NodePort
  ports:
  - port: 80          # Puerto interno del Service
    targetPort: 8080  # Puerto del contenedor
    nodePort: 30080   # Puerto expuesto en cada nodo
```

**Flujo de Red Completo:**
1. Cliente → Node_IP:30080 (NodePort)
2. Node → Service_IP:80 (ClusterIP)
3. Service → Pod_IP:8080 (TargetPort)

**LoadBalancer:**
- Extiende NodePort con un balanceador externo
- El proveedor cloud asigna una IP externa
- Flujo: External_IP → NodePort → Service → Pod

### Descubrimiento de Servicios

Kubernetes proporciona DNS interno:
- Cada Service es accesible por su nombre: `mi-servicio`
- FQDN completo: `mi-servicio.namespace.svc.cluster.local`
- Los Pods pueden resolver estos nombres automáticamente

## Ejemplo Práctico: Despliegue de Aplicación Web

### Paso 1: Deployment Configuration

Crear archivo `web-deployment.yaml`:

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: web-application
  labels:
    app: web-app
spec:
  replicas: 3
  selector:
    matchLabels:
      app: web-app
  template:
    metadata:
      labels:
        app: web-app
    spec:
      containers:
      - name: web-container
        image: nginx:1.21
        ports:
        - containerPort: 80
        resources:
          requests:
            memory: "64Mi"
            cpu: "250m"
          limits:
            memory: "128Mi"
            cpu: "500m"
```

### Paso 2: Service Configuration

Crear archivo `web-service.yaml`:

```yaml
apiVersion: v1
kind: Service
metadata:
  name: web-service
spec:
  type: NodePort
  selector:
    app: web-app
  ports:
  - protocol: TCP
    port: 80
    targetPort: 80
    nodePort: 30080
```

### Paso 3: Despliegue

```bash
# Aplicar configuraciones
kubectl apply -f web-deployment.yaml
kubectl apply -f web-service.yaml

# Verificar despliegue
kubectl get deployments
kubectl get pods -l app=web-app
kubectl get services
```

### Paso 4: Verificación de Red

```bash
# Ver detalles del Service
kubectl describe service web-service

# Ver IPs asignadas a los Pods
kubectl get pods -l app=web-app -o wide

# Probar conectividad interna (desde otro Pod)
kubectl run test-pod --image=busybox --rm -it -- sh
# Dentro del Pod de test:
# nslookup web-service
# wget -qO- web-service:80
```

### Paso 5: Acceso Externo

```bash
# Método 1: Port forwarding (desarrollo)
kubectl port-forward service/web-service 8080:80
# Acceder a http://localhost:8080

# Método 2: NodePort (Minikube)
minikube service web-service --url

# Método 3: IP del nodo + NodePort
# Obtener IP del nodo
kubectl get nodes -o wide
# Acceder a http://<node-ip>:30080
```

## Operaciones de Ciclo de Vida

### Escalado
```bash
# Escalar horizontalmente
kubectl scale deployment web-application --replicas=5

# Verificar escalado
kubectl get pods -l app=web-app

# Observar en tiempo real
kubectl get pods -l app=web-app -w
```

### Actualización Rolling
```bash
# Actualizar imagen
kubectl set image deployment/web-application web-container=nginx:1.22

# Monitorear rollout
kubectl rollout status deployment/web-application

# Ver historial
kubectl rollout history deployment/web-application

# Rollback si es necesario
kubectl rollout undo deployment/web-application
```

### Debugging
```bash
# Logs de aplicación
kubectl logs deployment/web-application

# Logs de un Pod específico
kubectl logs <pod-name>

# Ejecutar comandos en un Pod
kubectl exec -it <pod-name> -- /bin/bash

# Ver eventos del cluster
kubectl get events --sort-by=.metadata.creationTimestamp
```

## Limpieza de Recursos

```bash
# Eliminar por archivos
kubectl delete -f web-service.yaml
kubectl delete -f web-deployment.yaml

# Eliminar por nombre
kubectl delete service web-service
kubectl delete deployment web-application

# Verificar limpieza
kubectl get all -l app=web-app
```

## Mejores Prácticas

### Organización de Recursos
- Usar labels consistentes para agrupación y selección
- Aplicar límites de recursos para prevenir consumo excesivo
- Separar configuraciones en archivos independientes

### Gestión de Configuración
- Usar ConfigMaps para configuración no sensible
- Usar Secrets para información confidencial
- Implementar health checks (readiness y liveness probes)

### Monitoreo y Observabilidad
- Implementar logging estructurado
- Configurar métricas de aplicación
- Establecer alertas para recursos críticos
