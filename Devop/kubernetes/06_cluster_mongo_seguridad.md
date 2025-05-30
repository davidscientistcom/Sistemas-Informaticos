# MongoDB en Kubernetes — Exposición Externa y Seguridad del Clúster

##  Objetivo

Este documento extiende el despliegue anterior de un Replica Set MongoDB en Kubernetes, añadiendo dos aspectos clave para un entorno real:

1. **Exposición externa**: cómo acceder a MongoDB desde fuera del clúster
2. **Seguridad**: cómo configurar autenticación con usuario/contraseña y usar `Secrets`

> El objetivo es entender no solo *cómo* se hace, sino *por qué* se hace así en entornos de producción.



##  1. Exponer MongoDB al exterior del clúster

Por defecto, los servicios en Kubernetes solo están disponibles dentro del clúster. Si queremos acceder a MongoDB desde nuestro equipo local (por ejemplo, para usar Compass o una app), tenemos varias opciones:

### Opción A: `NodePort`

Archivo: `mongo-external-service.yaml`

```yaml
apiVersion: v1
kind: Service
metadata:
  name: mongo-external
spec:
  type: NodePort
  selector:
    app: mongo
  ports:
    - protocol: TCP
      port: 27017
      targetPort: 27017
      nodePort: 30017
```

**¿Por qué `NodePort`?**

* Es la forma más directa de exponer un puerto sin necesidad de Ingress o LoadBalancer
* El puerto `30017` será accesible desde la IP de tu clúster

```bash
kubectl apply -f mongo-external-service.yaml
minikube service mongo-external --url
```

> En Minikube, esto abrirá directamente MongoDB en el navegador o en una herramienta externa como Compass usando esa URL.



## 🔐 2. Añadir autenticación a MongoDB

MongoDB permite activar autenticación de usuarios usando el flag `--auth`. Para ello debemos:

1. Crear un usuario admin
2. Activar el modo seguro con `--auth`
3. Almacenar credenciales como `Secrets`

### Paso 1: Crear los `Secrets`

```bash
kubectl create secret generic mongo-auth \
  --from-literal=MONGO_INITDB_ROOT_USERNAME=admin \
  --from-literal=MONGO_INITDB_ROOT_PASSWORD=secreto123
```

### Paso 2: Usar los secretos en el StatefulSet

Editamos el contenedor de `mongo` en `mongo-statefulset.yaml`:

```yaml
env:
- name: MONGO_INITDB_ROOT_USERNAME
  valueFrom:
    secretKeyRef:
      name: mongo-auth
      key: MONGO_INITDB_ROOT_USERNAME
- name: MONGO_INITDB_ROOT_PASSWORD
  valueFrom:
    secretKeyRef:
      name: mongo-auth
      key: MONGO_INITDB_ROOT_PASSWORD
args: ["--replSet", "rs0", "--bind_ip_all", "--auth"]
```

> La clave aquí es pasar las variables como entorno y activar `--auth`.

### Paso 3: Reaplicar el StatefulSet

```bash
kubectl apply -f mongo-statefulset.yaml
```

Esto reiniciará los pods. Al acceder, necesitarás autenticarte:

```bash
kubectl exec -it mongo-0 -- mongo -u admin -p secreto123
```

Y crear una base de datos real desde la consola:

```js
db = db.getSiblingDB("admin")
db.createUser({
  user: "admin",
  pwd: "secreto123",
  roles: [ { role: "root", db: "admin" } ]
})
```



##  3. Conectar desde fuera del clúster

Si usamos `NodePort` y la IP del clúster es `192.168.49.2`, podremos conectar con:

```
mongodb://admin:secreto123@192.168.49.2:30017
```

Puedes usar esta URL en:

* MongoDB Compass
* Aplicaciones cliente
* Terminal (con `mongo` si está instalado localmente)



##  4. Consideraciones de seguridad

* Nunca incluyas contraseñas en texto plano en los YAML
* Usa `Secrets` y accede a ellos como variables de entorno
* Siempre activa `--auth` en producción
* Puedes añadir TLS y certificados en producción (fuera del alcance de este artefacto)



##  5. Verificación final

* Accede al servicio externo con Mongo Compass
* Autentícate con el usuario `admin`
* Inserta y consulta datos desde la app cliente
* Simula caída del primary y verifica que sigue funcionando



##  Conclusión

Ahora tienes un clúster MongoDB:

* Segurizado con autenticación
* Expuesto para su uso desde fuera del clúster
* Listo para integrarse con aplicaciones reales o entornos de pruebas
