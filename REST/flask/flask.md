### **Capítulo 1: Introducción a Flask para REST**

#### **1.1 ¿Qué es Flask?**
Flask es un microframework para el desarrollo de aplicaciones web en Python. Se considera "micro" porque ofrece las funcionalidades básicas para desarrollar aplicaciones web, sin incorporar herramientas complejas de frontend, manejo de formularios, o plantillas. Esto lo hace especialmente útil para construir APIs ligeras y rápidas, como los servicios REST.

#### **1.2 Instalación de Flask y configuración del entorno**

Antes de iniciar el desarrollo de un servicio REST en Flask, necesitamos configurar el entorno y asegurarnos de que tenemos Flask instalado. A continuación, se detallan los pasos necesarios:

##### Paso 1: Crear un entorno virtual

Es recomendable aislar los proyectos en entornos virtuales para gestionar las dependencias de cada proyecto sin interferir con otros proyectos. Para crear un entorno virtual, sigue los siguientes pasos en tu terminal:

```bash
# Crea un entorno virtual llamado "venv"
python3 -m venv venv

# Activa el entorno virtual
source venv/bin/activate  # En Linux o MacOS
# o
venv\Scripts\activate      # En Windows
```

##### Paso 2: Instalar Flask

Con el entorno virtual activado, instala Flask usando `pip`:

```bash
pip install Flask
```

Flask se instalará junto con sus dependencias, lo cual te permitirá empezar a desarrollar tu API.

---

### **Capítulo 2: Estructura Básica de una API REST en Flask**

#### **2.1 Creación y configuración inicial de un proyecto Flask**

1. **Estructura del Proyecto**

   Para mantener la organización en un proyecto Flask básico, utilizaremos la siguiente estructura de carpetas y archivos:

   ```
   my_flask_api/
   ├── app.py               # Archivo principal de la aplicación Flask
   ├── venv/                # Entorno virtual
   └── requirements.txt     # Dependencias del proyecto (opcional)
   ```

   - `app.py`: Contendrá el código principal de nuestra aplicación Flask.
   - `venv/`: Carpeta que contiene el entorno virtual (creada en el paso anterior).
   - `requirements.txt`: Archivo donde se listan las dependencias del proyecto (podemos crearlo más adelante con `pip freeze > requirements.txt`).

2. **Creación de la aplicación Flask en `app.py`**

   A continuación, crearemos una aplicación mínima en Flask en el archivo `app.py`. Este archivo será el núcleo de nuestra API y contendrá el código para configurar las rutas y métodos HTTP.

   ```python
   from flask import Flask

   # Inicialización de la aplicación Flask
   app = Flask(__name__)

   # Ruta básica
   @app.route('/')
   def home():
       return "Bienvenido a la API REST con Flask"

   if __name__ == "__main__":
       app.run(debug=True)
   ```

   - **Explicación del Código**:
     - `Flask(__name__)`: Este comando crea una instancia de la aplicación Flask. El parámetro `__name__` indica el nombre del módulo actual.
     - `@app.route('/')`: Define una ruta (o endpoint) para la URL base (`/`). Cada ruta está asociada a una función de Python que devuelve la respuesta para esa ruta.
     - `app.run(debug=True)`: Inicia la aplicación Flask en modo de depuración (debug), lo que proporciona información detallada en caso de errores y permite recargar automáticamente la aplicación en caso de cambios.

3. **Ejecución de la aplicación**

   Para ejecutar la aplicación Flask, dirígete a la carpeta del proyecto y usa el siguiente comando en la terminal:

   ```bash
   python app.py
   ```

   Deberías ver un mensaje indicando que la aplicación se está ejecutando en `http://127.0.0.1:5000/`. Puedes abrir esta URL en tu navegador o usar una herramienta como `curl` o Postman para verificar que la aplicación esté respondiendo.

   ```bash
   curl http://127.0.0.1:5000/
   ```

   Esto debería devolver la respuesta:

   ```
   Bienvenido a la API REST con Flask
   ```

---

### **Capítulo 3: Métodos HTTP en Flask**

#### **3.1 Introducción a los Métodos HTTP**

En una API REST, cada método HTTP realiza una operación específica sobre un recurso:
- **GET**: Recuperar datos del servidor.
- **POST**: Enviar datos al servidor.
- **PUT**: Actualizar un recurso existente.
- **DELETE**: Eliminar un recurso.

A continuación, veremos cómo implementar cada método en Flask.

---

#### **3.2 Implementación de GET en Flask**

El método GET se utiliza para recuperar información. Empezaremos creando una ruta que responda a solicitudes GET y devuelva datos en formato JSON.

##### Ejemplo básico: Ruta GET

1. **Añadir una ruta GET a `app.py`**

   ```python
   from flask import Flask, jsonify

   app = Flask(__name__)

   @app.route('/api/message', methods=['GET'])
   def get_message():
       return jsonify({"message": "Hola, este es un mensaje desde la API con método GET"})
   ```

   **Explicación**:
   - `@app.route('/api/message', methods=['GET'])`: Define una ruta específica `/api/message` y especifica que solo responde al método GET.
   - `jsonify`: Convierte el diccionario en una respuesta JSON. Este método es esencial para las API REST, ya que devuelve los datos en un formato estándar que los clientes pueden consumir fácilmente.

2. **Ejemplo Avanzado: Retornar una lista de usuarios**

   Supongamos que queremos crear una lista de usuarios y devolverla como respuesta. Agregamos el siguiente código a `app.py`:

   ```python
   users = [
       {"id": 1, "name": "Alice", "email": "alice@example.com"},
       {"id": 2, "name": "Bob", "email": "bob@example.com"},
   ]

   @app.route('/api/users', methods=['GET'])
   def get_users():
       return jsonify(users)
   ```

   - **Prueba de la Ruta**:
     Utiliza `curl` o Postman para hacer una solicitud GET a `/api/users`:

     ```bash
     curl http://127.0.0.1:5000/api/users
     ```

     La respuesta debería ser algo similar a:

     ```json
     [
         {"id": 1, "name": "Alice", "email": "alice@example.com"},
         {"id": 2, "name": "Bob", "email": "bob@example.com"}
     ]
     ```

#### **3.3 Implementación de POST en Flask**

El método POST permite enviar datos al servidor. Veamos cómo podemos implementar un endpoint que reciba datos de un cliente y los procese.

1. **Ruta POST para crear un nuevo usuario**

   ```python
   from flask import request

   @app.route('/api/users', methods=['POST'])
   def create_user():
       user = request.get_json()  # Captura los datos en formato JSON
       user['id'] = len(users) + 1
       users.append(user)
       return jsonify(user), 201  # Devuelve el nuevo usuario con código de estado 201
   ```

   - **Explicación**:
     - `request.get_json()`: Extrae los datos en formato JSON de la solicitud.
     - `user['id'] = len(users) + 1`: Asigna un ID al nuevo usuario.
     - `users.append(user)`: Agrega el nuevo usuario a la lista `users`.
     - `return jsonify(user), 201`: Devuelve el nuevo usuario y el código de estado 201 (`Created`), indicando que el recurso se ha creado correctamente.
