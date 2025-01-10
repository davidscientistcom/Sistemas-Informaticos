### 1. **Introducción a Ansible**

#### 1.1. ¿Qué es Ansible?

**Ansible** es una herramienta de código abierto para la automatización de la administración de sistemas. Fue diseñada para ser simple, potente y sin necesidad de instalar agentes en las máquinas que gestiona. Ansible permite:

- **Configurar sistemas** de manera uniforme.
- **Desplegar aplicaciones** con un solo comando.
- **Orquestar tareas complejas**, como actualizaciones de múltiples servidores.

Es especialmente popular en entornos de TI por su facilidad de uso y su capacidad de integrarse con sistemas existentes mediante el uso de **SSH**.

---

#### 1.2. ¿Por qué usar Ansible?

Ansible se destaca por:

1. **Facilidad de uso**: Usa un lenguaje declarativo basado en YAML que es intuitivo y fácil de aprender.
2. **Sin agentes**: No requiere la instalación de software adicional en los nodos gestionados, lo que reduce la sobrecarga.
3. **Escalabilidad**: Desde la gestión de un servidor hasta cientos o miles, Ansible escala fácilmente.
4. **Compatibilidad multiplataforma**: Funciona con diferentes sistemas operativos (Linux, Windows, macOS).
5. **Gran comunidad y soporte**: Su adopción masiva ha generado una rica documentación y contribuciones.

Ejemplo práctico:
Imagina que tienes 10 servidores y necesitas instalar Apache en todos ellos. Con Ansible, podrías hacerlo con un solo comando, en lugar de repetir el proceso manualmente en cada máquina.

---

#### 1.3. Conceptos clave

1. **Infraestructura como Código (IaC)**:
   - Ansible permite definir la configuración de tu infraestructura como código. Esto facilita la reproducibilidad, el control de versiones y el despliegue en diferentes entornos.

2. **Configuración centralizada**:
   - Desde una máquina principal (el **nodo controlador**), puedes gestionar múltiples servidores (los **nodos gestionados**).

3. **Automatización sin agentes**:
   - A diferencia de otras herramientas, Ansible no necesita que instales software en los nodos gestionados. Solo necesita acceso por **SSH** y Python.

---

#### 1.4. Casos de uso en la industria

1. **Gestión de configuraciones**:
   - Definir y mantener la configuración de servidores. Ejemplo: establecer la misma versión de Apache en todos los nodos.

2. **Despliegue de aplicaciones**:
   - Automatizar el despliegue de aplicaciones web. Ejemplo: desplegar una aplicación Django con una base de datos PostgreSQL.

3. **Orquestación**:
   - Coordinar tareas complejas que involucran múltiples servidores. Ejemplo: actualizar un sistema distribuido sin interrumpir el servicio.

4. **Seguridad y cumplimiento**:
   - Asegurar que los servidores cumplan con políticas de seguridad. Ejemplo: configurar reglas de firewall de manera uniforme.

