# MongoDB en Kubernetes — Backups Automáticos e Inicialización del Replica Set

##  Objetivo

En este documento aprenderemos a:

1. Automatizar la inicialización del Replica Set sin tener que entrar manualmente a `mongo-0`
2. Configurar **tareas de backup periódico** de MongoDB en Kubernetes usando `CronJob`

Esto permite tener un despliegue más profesional, reproducible y preparado para recuperación ante fallos graves.



##  1. ¿Por qué automatizar la inicialización del Replica Set?

En el despliegue anterior, tuvimos que:

* Entrar manualmente al pod `mongo-0`
* Ejecutar `rs.initiate({...})`

Esto no es deseable en producción ni cuando reiniciamos todo desde cero. Queremos que **MongoDB configure el Replica Set automáticamente** si detecta que es la primera vez que se inicia.



## ⚙️ 2. Usar un Init Container para configurar el Replica Set

### ¿Qué es un `initContainer`?

Es un contenedor que se ejecuta **antes del contenedor principal**. Lo usaremos para lanzar el comando `rs.initiate()` solo si es necesario.

### Paso 1: Script de inicialización

Creamos un `init.js` que se monta dentro del pod:

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

### Paso 2: Configurar el `StatefulSet` para montar y ejecutar

Fragmento del YAML:

```yaml
initContainers:
- name: mongo-init
  image: mongo
  command:
    - mongo
    - --host
    - mongo-0.mongo:27017
    - -u
    - $(MONGO_INITDB_ROOT_USERNAME)
    - -p
    - $(MONGO_INITDB_ROOT_PASSWORD)
    - --authenticationDatabase
    - admin
    - /scripts/init.js
  envFrom:
    - secretRef:
        name: mongo-auth
  volumeMounts:
    - name: init-script
      mountPath: /scripts

volumes:
- name: init-script
  configMap:
    name: mongo-init-script
```

### Paso 3: Crear el ConfigMap con el script

```bash
kubectl create configmap mongo-init-script --from-file=init.js
```

> Este `initContainer` solo se ejecutará al arrancar los pods por primera vez. Si el Replica Set ya existe, Mongo ignorará el comando.



##  3. Configurar backups automáticos con `CronJob`

### ¿Qué es un `CronJob`?

Es un recurso de Kubernetes que ejecuta una tarea programada al estilo `cron` de Linux. Ideal para hacer backups diarios, semanales, etc.

### Paso 1: Crear un script de backup (opcional)

Si usamos `mongodump`, no hace falta crear nada especial:

### Paso 2: Definir el `CronJob`

Archivo: `mongo-backup-cronjob.yaml`

```yaml
apiVersion: batch/v1
kind: CronJob
metadata:
  name: mongo-backup
spec:
  schedule: "0 2 * * *"  # todos los días a las 2:00
  jobTemplate:
    spec:
      template:
        spec:
          containers:
          - name: backup
            image: mongo
            command:
              - /bin/sh
              - -c
              - |
                mongodump \
                  --uri="mongodb://admin:secreto123@mongo-0.mongo:27017/?replicaSet=rs0" \
                  --archive=/backup/backup-$(date +%Y%m%d%H%M).gz \
                  --gzip
            volumeMounts:
            - name: backup-volume
              mountPath: /backup
          restartPolicy: OnFailure
          volumes:
          - name: backup-volume
            hostPath:
              path: /data/mongo-backups
              type: DirectoryOrCreate
```

> En este ejemplo, los backups se guardan en el nodo local. En producción se usaría un volumen en la nube o sistema de almacenamiento remoto (NFS, S3, etc).

### Verificar que funciona:

```bash
kubectl get cronjob
kubectl get jobs --watch
```

Los archivos `.gz` deberían aparecer en `/data/mongo-backups` del nodo donde se ejecute.



##  4. Buenas prácticas para backup y restauración

* No hagas dump de bases de datos con usuarios conectados en producción sin bloquear operaciones críticas
* Usa etiquetas o nombres con fechas para organizar backups
* Automatiza limpieza con `RetentionPolicy` si generas muchos archivos
* En entornos reales: usa `Velero`, `Restic` o copia a un bucket de S3



##  5. Verificación final

* Reinstala el StatefulSet desde cero: verifica que el init container configura automáticamente el Replica Set
* Consulta los `jobs` generados por el CronJob y verifica archivos de backup



##  Conclusión

Este artefacto permite:

* Tener una base de datos inicializable automáticamente sin pasos manuales
* Ejecutar backups programados de forma estable y limpia

En el siguiente artefacto abordaremos:

* Monitoreo básico del estado de MongoDB
* Exportación de métricas
* Visualización con Prometheus y Grafana
