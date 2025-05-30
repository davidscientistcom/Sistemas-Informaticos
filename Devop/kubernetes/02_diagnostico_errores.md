# Explicación 2: Diagnóstico de Errores Comunes y Buenas Prácticas en Kubernetes

##  Introducción

Desplegar un contenedor en Kubernetes no es suficiente. Lo importante es saber **reaccionar cuando algo falla**, interpretar el sistema y mejorar la calidad de nuestras definiciones YAML. En este documento te explicamos **los errores típicos**, cómo diagnosticarlos y cómo anticiparse a ellos con buenas prácticas.


## 1.  Errores frecuentes en archivos YAML

### a) **Errores de sintaxis o indentación**

Kubernetes es muy estricto con los espacios. Un `spec` mal indentado o un `:` sin espacio puede invalidar el fichero.

**Solución:** Validar con `--dry-run`:

```bash
kubectl apply -f archivo.yaml --dry-run=client
```

### b) **Campos mal colocados**

Ejemplo típico: poner `containers:` al mismo nivel que `spec:` en lugar de dentro de `template.spec`.

**Solución:** Leer la documentación oficial:

```bash
kubectl explain deployment.spec.template.spec.containers
```



## 2.  Errores comunes al aplicar recursos

### a) **Recurso ya existe**

```bash
kubectl apply -f nginx-deployment.yaml
```

> `Error from server (AlreadyExists)`

**Solución:**

* Reemplazar: `kubectl replace -f archivo.yaml`
* Eliminar y aplicar: `kubectl delete -f archivo.yaml && kubectl apply -f archivo.yaml`

### b) **Label incorrecto en el Service**

El `Service` no encuentra ningún `Pod` porque el `selector:` no coincide con las `labels` de los `Pods`.

**Diagnóstico:**

```bash
kubectl get pods --show-labels
kubectl describe service nginx-service
```



## 3. Diagnóstico de fallos en pods

### a) **Ver detalles del pod**

```bash
kubectl describe pod POD_NAME
```

Revisa:

*  Errores en el scheduling
*  Imagen no encontrada
*  Permisos denegados

### b) **Ver los logs del contenedor**

```bash
kubectl logs POD_NAME
```

Si el pod se reinicia en bucle („CrashLoopBackOff), los logs te dirán por qué falló (config, puertos, comandos...)

### c) **Ver ubicación de los pods**

```bash
kubectl get pods -o wide
```

Te ayuda a saber si están todos en el mismo nodo o si hay problemas de afinidad.



## 4. 🔎 Diagnóstico de fallos en el Service

### a) **Puerto mal definido**

* `port` y `targetPort` no deben estar intercambiados
* Verifica que el `containerPort` en el pod coincide con el `targetPort` del `Service`

### b) **El tipo NodePort no genera URL**

```bash
minikube service nginx-service --url
```

Si no devuelve nada:

* El `Service` no está listo
* Revisa `type`, `selector`, y que los pods estén corriendo



## 5. ⚡ Testing y depuración desde dentro del clúcster

### a) Ejecutar un pod de prueba

```bash
kubectl run testpod --rm -it --image=busybox -- /bin/sh
```

Desde ese pod puedes:

* `ping nginx-service`
* `wget nginx-service`
* `nslookup nginx-service`

### b) Comprobar eventos recientes

```bash
kubectl get events --sort-by=.metadata.creationTimestamp
```

Esto muestra errores recientes (fallos al montar volumen, errores DNS, fallos de scheduling...)

## 6.  Buenas prácticas en YAML y operación

### a) Dividir ficheros por responsabilidad

* `nginx-deployment.yaml`
* `nginx-service.yaml`
* `nginx-configmap.yaml` (si hace falta configuración)

### b) Versionar tus archivos con Git

* Permite retroceder a versiones anteriores
* Comparar cambios entre versiones

### c) Verifica que tus YAMLs son idempotentes

* Puedes aplicarlos varias veces sin causar errores o efectos no deseados



## 7. ❓ ¿Y si no tengo ni idea de qué está fallando?

Checklist rápido:

1.  `kubectl get all` → ¿hay pods corriendo?
2.  `kubectl describe` al `pod` y al `service`
3.  Ver `kubectl logs` del pod
4.  Ejecutar un pod de test y hacer `curl` al servicio
5.  Si usas Minikube:

```bash
minikube service nginx-service
```



## 8. 💡 Consejos reales para el mundo profesional

* Aprende a usar `kubectl` como una herramienta de diagnóstico, no solo de ejecución
* No edites YAMLs a ciegas: valida, explica y comprende cada sección
* Practica con fallos simulados: borra pods, cambia puertos, rompe etiquetas
* La frase mágica en entrevistas: **"Yo sigo un enfoque sistemático: primero verifico que el recurso está creado, luego que está corriendo, y finalmente que responde desde el exterior"**
