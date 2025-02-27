# **Práctica de PostgreSQL con Vagrant y Ansible: Conceptos Clave y Aplicaciones Prácticas**


## **1. ¿Por Qué Hacemos la Práctica de PostgreSQL?**

### **1.1. Objetivo Principal**
La práctica de PostgreSQL tiene como objetivo:
- **Configurar un servidor de base de datos** en una máquina virtual (VM) utilizando Vagrant.
- **Automatizar la instalación y configuración** con Ansible.
- **Permitir el acceso remoto** desde herramientas de administración, como DBeaver, para gestionar la base de datos desde fuera de la VM.

### **1.2. ¿Por Qué Usamos Vagrant y Ansible?**
- **Vagrant** se utiliza para **crear** y **gestionar** la VM de manera rápida y repetible.
- **Ansible** automatiza la **instalación** y **configuración** de PostgreSQL de forma **consistente** y **idempotente**, asegurando que siempre obtengas el mismo resultado sin importar cuántas veces ejecutes el script.



## **2. Acceso Remoto a PostgreSQL: ¿Por Qué y Cómo?**

### **2.1. ¿Por Qué Acceder de Forma Remota?**
- En entornos de desarrollo, es común querer **gestionar la base de datos** desde el host (tu máquina) utilizando herramientas gráficas como **DBeaver** o **pgAdmin**.
- Esto **facilita la administración**, ya que puedes navegar por las tablas, realizar consultas SQL y gestionar usuarios de forma más intuitiva.

### **2.2. ¿Cómo Permitimos el Acceso Remoto?**

Para permitir el acceso remoto de manera **segura** y **funcional**, se deben configurar tres cosas clave:
1. **Escuchar en todas las direcciones IP:** La configuración predeterminada de PostgreSQL solo escucha en `localhost`, lo cual impide el acceso remoto.
2. **Permitir conexiones en el firewall:** Necesitamos abrir el puerto 5432 (el puerto predeterminado de PostgreSQL) en el firewall de la VM.
3. **Configurar permisos de acceso en PostgreSQL:** Se debe permitir explícitamente el acceso remoto en el archivo `pg_hba.conf`.



### **2.3. Paso 1: Configuración en `postgresql.conf`**

Modificamos el archivo de configuración para que PostgreSQL **escuche en todas las direcciones IP**:

```conf
listen_addresses = '*'
```

- Esto permite que el servidor PostgreSQL acepte conexiones desde **cualquier dirección IP**.
- **¿Por qué es necesario?**: Si no se hace esto, PostgreSQL solo aceptará conexiones locales (dentro de la misma VM).



### **2.4. Paso 2: Configuración en `pg_hba.conf`**

Añadimos una regla de acceso para permitir **conexiones remotas**:

```conf
host    all             all             0.0.0.0/0               md5
```

- **`0.0.0.0/0`** indica que cualquier IP puede conectarse.
- **`md5`** especifica que la autenticación será mediante contraseña encriptada.

- **¿Por qué es necesario?**: Si no se añade esta línea, PostgreSQL rechazará cualquier conexión remota por seguridad.



### **2.5. Paso 3: Apertura del Puerto en el Firewall**

Ejecutamos el siguiente comando en la VM:

```sh
sudo ufw allow 5432/tcp
```

- Esto **permite conexiones entrantes** en el puerto `5432` usando el protocolo TCP.
- **¿Por qué es necesario?**: Si el puerto está cerrado, el host no podrá alcanzar el servicio PostgreSQL en la VM.



### **2.6. ¿Por Qué No Usamos Port Forwarding de Vagrant?**

En lugar de usar `config.vm.network "forwarded_port"`, usamos una **Red Privada (Host-Only)** con una **IP estática** y abrimos el puerto directamente en la VM. 

**Razones:**
- **Evitar conflictos de puertos:** Al no tener que mapear el puerto 5432 a otro puerto en el host, evitamos conflictos con otros servicios locales.
- **Simulación de un entorno de red real:** La VM obtiene una dirección IP fija, como si fuera un servidor en una red local real.
- **Acceso directo a la IP estática:** Se accede a PostgreSQL usando la **IP fija** de la VM (`192.168.33.10` en el ejemplo), no `localhost`.



### **2.7. Conexión desde DBeaver**

Una vez configurado PostgreSQL y abierto el puerto en la VM, puedes conectarte desde **DBeaver** en tu máquina host con los siguientes datos:

- **Host:** `192.168.33.10` (la IP fija de la VM)
- **Puerto:** `5432`
- **Usuario:** `david`
- **Contraseña:** `david`
- **Base de Datos:** `taller`



## **3. ¿Por Qué Usar Ansible en Lugar de `Vagrant Provision`?**

### **3.1. `Vagrant Provision`: Limitaciones y Desventajas**

- **Uso de Shell Scripts:** Aunque permite usar scripts shell (`config.vm.provision "shell"`), estos:
  - Son **imperativos**, lo que hace que se vuelvan complejos y difíciles de mantener a medida que crecen.
  - No son **idempotentes**: Si ejecutas el `Vagrantfile` varias veces, el script shell se ejecuta de nuevo, incluso si ya se ha completado la tarea (por ejemplo, volvería a instalar PostgreSQL).

- **Repetibilidad y Mantenibilidad Limitada:**
  - No hay una manera fácil de reutilizar configuraciones en diferentes `Vagrantfile`.
  - La configuración se mezcla con la definición de la máquina virtual, lo que reduce la claridad y modularidad.



### **3.2. Ventajas de Usar Ansible**

- **Idempotencia:**
  - Ansible verifica el estado actual de la configuración antes de aplicar cambios.
  - Ejemplo: Si PostgreSQL ya está instalado, no lo reinstala.

- **Legibilidad y Mantenibilidad:**
  - La configuración se define en **YAML**, lo cual es más legible y estructurado.
  - Es fácil dividir configuraciones en **roles** y reutilizarlas en múltiples proyectos.

- **Separación de Preocupaciones:**
  - Vagrant **crea la infraestructura** (la VM), mientras que Ansible **configura el software**.
  - Esto sigue el principio de **Infraestructura como Código** (IaC).



## **4. Ejemplos Prácticos y Casos de Uso Comunes**

### **4.1. Caso 1: Desarrollo y Pruebas en Bases de Datos**

- **Escenario:** Desarrolladores trabajando en una aplicación que utiliza PostgreSQL.
- **Uso de Vagrant y Ansible:**
  - Se crea una VM con PostgreSQL configurado y accesible desde las estaciones de trabajo de los desarrolladores.
  - Esto permite probar en un entorno aislado antes de desplegar en producción.
- **Beneficios:**
  - Consistencia en el entorno de pruebas.
  - Acceso remoto desde herramientas como **DBeaver** sin necesidad de instalar PostgreSQL localmente.



### **4.2. Caso 2: Entornos de Pruebas para DevOps**

- **Escenario:** Pruebas de despliegue continuo (CI/CD) con bases de datos en diferentes versiones.
- **Uso de Vagrant y Ansible:**
  - Se pueden crear múltiples VMs con diferentes versiones de PostgreSQL.
  - Ansible asegura que la configuración (usuarios, permisos, datos de prueba) sea **idéntica** en todas las versiones.
- **Beneficios:**
  - Pruebas de compatibilidad con diferentes versiones de PostgreSQL.
  - Automatización completa de la configuración y despliegue.



### **4.3. Caso 3: Formación y Aprendizaje de Bases de Datos**

- **Escenario:** Cursos o talleres sobre PostgreSQL.
- **Uso de Vagrant y Ansible:**
  - Se entrega un `Vagrantfile` y un playbook de Ansible a los estudiantes.
  - Los estudiantes pueden levantar entornos idénticos en sus propias máquinas.
- **Beneficios:**
  - Simplificación del entorno de aprendizaje.
  - Garantía de que todos los estudiantes trabajan en el mismo entorno.

