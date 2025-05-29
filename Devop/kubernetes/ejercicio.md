# Ejercicio: Nginx en Kubernetes con Minikube (Windows)

## Prerrequisitos
- Minikube instalado en Windows
- kubectl configurado
- Docker Desktop (recomendado)

## Paso 1: Verificar que Minikube está funcionando

```bash
# Iniciar Minikube (si no está corriendo)
minikube start

# Verificar el estado
minikube status

# Verificar que kubectl está conectado
kubectl cluster-info
```

## Paso 2: Crear el Deployment de nginx

Crea un archivo llamado `nginx-deployment.yaml`:

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: nginx-deployment
  labels:
    app: nginx
spec:
  replicas: 3
  selector:
    matchLabels:
      app: nginx
  template:
    metadata:
      labels:
        app: nginx
    spec:
      containers:
      - name: nginx
        image: nginx:latest
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

## Paso 3: Crear el Service para exponer nginx

Crea un archivo llamado `nginx-service.yaml`:

```yaml
apiVersion: v1
kind: Service
metadata:
  name: nginx-service
spec:
  selector:
    app: nginx
  ports:
    - protocol: TCP
      port: 80
      targetPort: 80
  type: NodePort
```

## Paso 4: Desplegar nginx

```bash
# Aplicar el deployment
kubectl apply -f nginx-deployment.yaml

# Aplicar el service
kubectl apply -f nginx-service.yaml

# Verificar que los pods están corriendo
kubectl get pods -l app=nginx

# Verificar el deployment
kubectl get deployment nginx-deployment

# Ver detalles del deployment
kubectl describe deployment nginx-deployment
```

## Paso 5: Verificar que todo funciona

```bash
# Ver los pods con más detalle
kubectl get pods -l app=nginx -o wide

# Ver los servicios
kubectl get services

# Obtener la URL del servicio en Minikube
minikube service nginx-service --url

# También puedes abrir directamente en el navegador
minikube service nginx-service
```

## Paso 6: Monitorear los logs

```bash
# Ver logs de todos los pods nginx
kubectl logs -l app=nginx

# Ver logs de un pod específico (reemplaza POD_NAME)
kubectl logs POD_NAME

# Seguir logs en tiempo real
kubectl logs -f -l app=nginx

# Ver logs de un pod específico en tiempo real
kubectl logs -f POD_NAME
```

## Paso 7: Simular fallo - Eliminar un pod

```bash
# Listar los pods actuales
kubectl get pods -l app=nginx

# Eliminar un pod específico (reemplaza POD_NAME por el nombre real)
kubectl delete pod POD_NAME

# Inmediatamente después, verificar el estado
kubectl get pods -l app=nginx

# Ver eventos del deployment
kubectl describe deployment nginx-deployment
```

### ¿Qué debería pasar?

1. **Antes de eliminar**: Tienes 3 pods corriendo
2. **Al eliminar un pod**: Kubernetes detecta que solo hay 2 pods
3. **Kubernetes reacciona**: Automáticamente crea un nuevo pod para mantener las 3 réplicas
4. **Estado final**: Vuelves a tener 3 pods corriendo

## Paso 8: Observar la recuperación automática

```bash
# Monitorear en tiempo real los cambios
kubectl get pods -l app=nginx -w

# En otra terminal, elimina un pod y observa la recuperación
kubectl delete pod POD_NAME

# Ver logs del controlador de réplicas
kubectl logs -n kube-system -l component=kube-controller-manager
```

## Paso 9: Experimentos adicionales

### Escalar el deployment
```bash
# Aumentar a 5 réplicas
kubectl scale deployment nginx-deployment --replicas=5

# Verificar
kubectl get pods -l app=nginx

# Volver a 3 réplicas
kubectl scale deployment nginx-deployment --replicas=3
```

### Ver logs de eventos
```bash
# Ver eventos del cluster
kubectl get events --sort-by=.metadata.creationTimestamp

# Ver eventos específicos del namespace
kubectl get events --field-selector involvedObject.name=nginx-deployment
```

### Probar la alta disponibilidad
```bash
# Eliminar múltiples pods a la vez
kubectl delete pods -l app=nginx --grace-period=0

# Observar cómo Kubernetes los recrea
kubectl get pods -l app=nginx -w
```

## Paso 10: Limpiar el entorno

```bash
# Eliminar los recursos creados
kubectl delete -f nginx-service.yaml
kubectl delete -f nginx-deployment.yaml

# Verificar que se eliminaron
kubectl get pods -l app=nginx
kubectl get services nginx-service
```

## Comandos útiles para debugging

```bash
# Información detallada de un pod
kubectl describe pod POD_NAME

# Acceder a un pod (shell interactivo)
kubectl exec -it POD_NAME -- /bin/bash

# Ver el uso de recursos
kubectl top pods -l app=nginx

# Ver configuración del deployment en YAML
kubectl get deployment nginx-deployment -o yaml
```

