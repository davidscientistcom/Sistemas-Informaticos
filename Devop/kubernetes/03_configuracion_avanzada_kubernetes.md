# Explicación 3: Configuración Avanzada en Kubernetes — Variables, ConfigMaps, y VolúMenes

##  Objetivo

En este documento aprenderemos a utilizar **configuraciones externas** para separar la lógica de despliegue de los datos y configuraciones sensibles o variables. Trabajaremos con:

* Variables de entorno
* ConfigMaps
* VolúMenes y montaje de archivos

Esto permite que nuestras aplicaciones sean **más reutilizables, seguras y profesionales**, y nos prepara para trabajar en entornos reales donde las configuraciones cambian entre desarrollo, staging y producción.



## 1. Uso de Variables de Entorno en los Pods

Puedes definir variables de entorno directamente dentro de un contenedor:

```yaml
env:
  - name: MI_VARIABLE
    value: "valor123"
```

### Ejemplo completo:

```yaml
containers:
- name: nginx
  image: nginx
  env:
  - name: ENTORNO
    value: "desarrollo"
```

Desde dentro del contenedor podrías hacer:

```bash
echo $ENTORNO
```



## 2. 📁 ConfigMaps: configuración externa reutilizable

Un **ConfigMap** permite guardar pares clave-valor y archivos de configuración que luego puedes inyectar a los pods como variables o ficheros.

### a) Crear un ConfigMap desde fichero

```bash
kubectl create configmap nginx-config --from-file=default.conf
```

### b) Crear un ConfigMap desde el YAML:

```yaml
apiVersion: v1
kind: ConfigMap
metadata:
  name: ejemplo-config
  labels:
    app: nginx
data:
  ENTORNO: "produccion"
  PUERTO: "8080"
```

### c) Usar ConfigMap como variables de entorno

```yaml
envFrom:
- configMapRef:
    name: ejemplo-config
```

Esto inyecta todas las claves del ConfigMap como variables.



## 3. 🌀 Montaje de ConfigMap como archivo

Puedes montar el ConfigMap como un volumen y acceder a sus claves como archivos individuales dentro del contenedor:

```yaml
volumeMounts:
- name: config-vol
  mountPath: /etc/config
volumes:
- name: config-vol
  configMap:
    name: ejemplo-config
```

Esto crea dentro del contenedor:

* `/etc/config/ENTORNO` con contenido "produccion"
* `/etc/config/PUERTO` con contenido "8080"



## 4.  VolúMenes: persistencia y configuración

### a) EmptyDir (temporal mientras el pod vive)

```yaml
volumes:
- name: cache
  emptyDir: {}
```

### b) HostPath (solo en desarrollo con Minikube)

```yaml
volumes:
- name: host-vol
  hostPath:
    path: /data/pruebas
```

**⚠️ Advertencia:** No se usa en producción, pero es útil en tests.



## 5.  Ejemplo completo: Nginx con ConfigMap

### a) `default.conf` personalizado:

```nginx
server {
  listen 8080;
  location / {
    return 200 "Hola desde nginx configurado por ConfigMap!\n";
  }
}
```

### b) Crear ConfigMap

```bash
kubectl create configmap nginx-conf --from-file=default.conf
```

### c) Deployment YAML con montaje del config

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: nginx-configurado
spec:
  replicas: 1
  selector:
    matchLabels:
      app: nginx-config
  template:
    metadata:
      labels:
        app: nginx-config
    spec:
      containers:
      - name: nginx
        image: nginx
        ports:
        - containerPort: 8080
        volumeMounts:
        - name: config-vol
          mountPath: /etc/nginx/conf.d
      volumes:
      - name: config-vol
        configMap:
          name: nginx-conf
```



## 6. Buenas prácticas

* Nunca "hardcodees" configuraciones en los YAML del pod.
* Separa la **lógica de despliegue (deployment)** de la **configuración del entorno (ConfigMap)**.
* Usa `kubectl get configmap -o yaml` para auditar y versionar.
* Usa nombres claros y consistentes para los recursos.



Este artefacto te prepara para el siguiente paso: montar **Secrets para datos sensibles**, integrar con bases de datos y trabajar con despliegues reales multientorno. También es el primer paso hacia la **infrastructure as code** y la automatización CI/CD.
