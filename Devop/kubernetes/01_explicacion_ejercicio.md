# Explicación 1: Despliegue de Nginx en Kubernetes con Minikube — Sentido Operativo y Guía de Aprendizaje

## 🔍 Objetivo

Comprender operativamente qué significa desplegar un contenedor en Kubernetes y por qué se realizan ciertos pasos como crear un `Deployment` y un `Service`. Este documento acompaña el tutorial técnico y busca responder:

* ¿Qué estoy haciendo?
* ¿Por qué se hace así?
* ¿Cómo puedo aprender a hacerlo sin copiar y pegar comandos?



##  1. Entendiendo el encargo: "Despliega un contenedor"

Cuando te piden "desplegar un contenedor en Kubernetes", esperan que:

* Un contenedor con una imagen (ej. `nginx:latest`) se ejecute en el clúcster.
* Haya **varias réplicas** por resiliencia.
* Puedas **acceder a esa app** desde fuera del clúcster (al menos desde tu equipo local).

**Traducción a recursos de Kubernetes:**

* `Deployment`: se encarga de crear y mantener las réplicas del contenedor
* `Service`: expone esos pods para que puedan recibir tráfico



##  2. ¿Por qué se crea un Deployment?

El `Deployment` es un objeto de Kubernetes que se encarga de:

* Crear el número deseado de **pods** con el contenedor que indiques
* Garantizar que ese número se mantenga (si un pod falla, se crea otro)
* Permitir **actualizaciones seguras** de la imagen (rolling updates)

### Ejemplo:

Si defines un `Deployment` con 3 réplicas de `nginx:latest`, Kubernetes:

* Crea 3 pods con ese contenedor
* Los monitoriza
* Si uno se borra, crea otro automáticamente (ver paso 7 del tutorial)



##  3. ¿Por qué se crea un Service?

Aunque los pods se creen, **no tienen IP fija**. Cambian si se reinician.

Por eso, el `Service` crea una **IP virtual estable** que siempre apunta a los pods correctos, usando el `selector` por etiquetas (ej. `app: nginx`).

El tipo `NodePort` permite acceder desde el exterior del clúster (desde Minikube, por ejemplo).



##  4. ¿Cómo aprendo a crear estos YAMLs si no me los dan?

### 1. Usando `kubectl create` con `--dry-run=client -o yaml`

```bash
kubectl create deployment nginx --image=nginx:latest --dry-run=client -o yaml > nginx-deployment.yaml
```

Esto genera un `Deployment` básico que puedes editar a mano.

### 2. Consultando la documentación oficial:

* [https://kubernetes.io/docs/concepts/](https://kubernetes.io/docs/concepts/)
* [https://kubernetes.io/docs/reference/kubernetes-api/](https://kubernetes.io/docs/reference/kubernetes-api/)

### 3. Usando ejemplos de otros repos:

* GitHub: busca `nginx deployment kubernetes yaml`

### 4. Autogenerando desde el dashboard (si está habilitado)



##  5. Esquema de flujo operativo.

**Ejemplo**: "Despliega nginx en Kubernetes con 3 réplicas accesibles desde navegador"

1. **Analizar el requerimiento**:

   * Imagen: nginx
   * Escalabilidad: 3 réplicas
   * Acceso: desde navegador = acceso externo

2. **Diseñar el despliegue**:

   * Crear un `Deployment` con 3 réplicas
   * Crear un `Service` tipo `NodePort`

3. **Aplicar los recursos**:

```bash
kubectl apply -f nginx-deployment.yaml
kubectl apply -f nginx-service.yaml
```

4. **Verificar funcionamiento**:

```bash
kubectl get pods
kubectl get services
minikube service nginx-service
```

5. **Monitorear y testear tolerancia a fallos**:

   * Borrar pods y ver regeneración
   * Cambiar número de réplicas con `kubectl scale`



##  6. Lección final: Operar con sentido

No es suficiente con saber los comandos. Lo importante es que:

* **Comprendas el porqué** de cada componente
* **Sépas leer YAMLs ajenos** y modificarlos
* **Detectes errores comunes** (puertos mal mapeados, selector mal escrito, etc.)
