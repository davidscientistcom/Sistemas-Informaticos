El archivo **`known_hosts`** es un componente importante de SSH que almacena las claves públicas de los servidores a los que tu sistema se ha conectado previamente. Vamos a explicar en detalle su función, cómo funciona dentro del ecosistema SSH y su relación con herramientas de automatización como Ansible.

### ¿Qué es `known_hosts`?

Cuando te conectas a un servidor por primera vez mediante SSH, el servidor envía su **clave pública** para identificarse. Tu sistema almacena esta clave en el archivo `known_hosts`, que se encuentra en `~/.ssh/` en tu sistema local. La próxima vez que intentes conectarte al mismo servidor, tu sistema comparará la clave pública que recibe con la que tiene almacenada en `known_hosts`. 

Si la clave coincide, SSH permite la conexión sin pedir confirmación. Si la clave ha cambiado o no existe en `known_hosts`, SSH mostrará una advertencia o un error, ya que esto podría significar un cambio en el servidor (por ejemplo, por una actualización de seguridad o un posible ataque).

### Ejemplo de entrada en `known_hosts`

Un registro típico en el archivo `known_hosts` se vería así:

```plaintext
192.168.0.45 ssh-rsa AAAAB3NzaC1yc2EAAAABIwAAAQEAmzCkl7JLlPf...
```

- **`192.168.0.45`** es la dirección IP del servidor o el nombre del dominio.
- **`ssh-rsa`** es el tipo de clave.
- La cadena larga de caracteres es la **clave pública del servidor**.

Cada vez que te conectas a un nuevo servidor, SSH agrega una nueva entrada en `known_hosts`.

### ¿Para qué sirve `known_hosts`?

El archivo `known_hosts` sirve principalmente para:
1. **Verificación de Identidad**: Asegura que estás conectándote al mismo servidor y no a un servidor malicioso que suplanta su identidad.
2. **Seguridad**: Detecta cambios en el servidor, como una posible intrusión o un cambio de configuración de seguridad.

### Relación entre `known_hosts` y Ansible

En el contexto de **Ansible**, el archivo `known_hosts` es importante porque Ansible utiliza SSH para gestionar conexiones y ejecutar comandos en servidores remotos. Si los hosts que Ansible intenta gestionar no están en `known_hosts`, puedes encontrarte con dos situaciones:

1. **Ansible arroja un error** indicando que no reconoce la clave del servidor.
2. **Ansible falla al conectarse** si el servidor ya está en `known_hosts` pero con una clave diferente (esto sucede si se ha reinstalado o cambiado la clave SSH en el servidor).

Para evitar estos problemas, Ansible permite configurar su comportamiento frente al archivo `known_hosts` en el archivo de configuración o mediante opciones específicas.

### Opciones de Ansible Relacionadas con `known_hosts`

1. **Saltear la verificación de `known_hosts`**:
   Puedes añadir la opción `-o StrictHostKeyChecking=no` a la configuración de Ansible para que no verifique las claves en `known_hosts`. Esto se configura en el archivo `ansible.cfg` o al lanzar el playbook. Ejemplo:

   ```ini
   [defaults]
   host_key_checking = False
   ```

   Esta configuración permite a Ansible conectarse a cualquier servidor nuevo sin verificar ni agregar claves a `known_hosts`, pero reduce la seguridad al desactivar la autenticación de la clave del host.

2. **Ansible y claves estáticas en `known_hosts`**:
   Puedes preconfigurar las claves de los servidores en `known_hosts` para asegurar que los servidores reconocidos tengan las claves adecuadas. Esto es especialmente útil en entornos donde tienes control total sobre los servidores y puedes gestionar las claves manualmente o mediante scripts.

3. **Limpiar entradas en `known_hosts` con Ansible**:
   Si quieres eliminar las entradas incorrectas en `known_hosts`, puedes crear un playbook en Ansible para ejecutar comandos como `ssh-keygen -R <host>` y limpiar el archivo en todos los nodos antes de conectarte.

### Ejemplo Práctico con Ansible

Aquí tienes un ejemplo de un playbook que añade las claves de varios hosts al archivo `known_hosts` de cada nodo gestionado, usando el módulo `known_hosts` de Ansible:

```yaml
---
- name: Asegurar claves conocidas en todos los nodos
  hosts: all
  tasks:
    - name: Agregar la clave SSH del host al archivo known_hosts
      ansible.builtin.known_hosts:
        path: /home/usuario/.ssh/known_hosts
        name: "192.168.0.45"
        key: "ssh-rsa AAAAB3NzaC1yc2EAAAABIwAAAQEAmzCkl7JLlPf..."
```

En este ejemplo, `known_hosts` de cada nodo tendrá una entrada específica para `192.168.0.45`, y esto evitará problemas de conexión.

### Resumen

El archivo `known_hosts` es fundamental para mantener la seguridad en conexiones SSH, permitiendo que tu sistema verifique la identidad de los servidores. En el contexto de Ansible, `known_hosts` juega un rol en la gestión segura de hosts, y su configuración o manejo adecuado evita problemas de conectividad o errores en la ejecución de playbooks.