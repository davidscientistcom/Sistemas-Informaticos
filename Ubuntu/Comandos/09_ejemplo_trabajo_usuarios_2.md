 Supongamos que trabajamos en una empresa llamada **Tech Solutions** y queremos implementar un sistema de archivos para su departamento de desarrollo.

### 1. Estructura de Directorios

La empresa quiere organizar sus archivos de la siguiente manera:

```
/home/techsolutions
├── proyectos
│   ├── proyecto_A
│   ├── proyecto_B
│   └── proyecto_C
├── recursos
│   ├── documentos
│   ├── plantillas
│   └── imagenes
└── personal
    ├── ingenieria
    ├── marketing
    └── administracion
```

### 2. Creación de Usuarios

Los empleados de Tech Solutions se dividen en varios departamentos, cada uno con su propio conjunto de permisos. Crearemos tres usuarios de ejemplo en Ubuntu para los diferentes departamentos y configuraremos sus contraseñas.

Supongamos que tenemos tres usuarios:

- **javier_dev**: ingeniero de desarrollo.
- **maria_marketing**: empleada del departamento de marketing.
- **ana_admin**: administradora del sistema.

Para crear estos usuarios con contraseñas, ejecutaríamos los siguientes comandos:

```bash
sudo adduser javier_dev
sudo adduser maria_marketing
sudo adduser ana_admin
```

Durante la creación, el sistema pedirá una contraseña para cada usuario y algunos datos adicionales (nombre, número de teléfono, etc.).

### 3. Asignación de Permisos de Acceso

Queremos que:

- **javier_dev** tenga acceso completo (lectura y escritura) a la carpeta `proyectos`.
- **maria_marketing** solo pueda leer el contenido en la carpeta `proyectos` y en `recursos`.
- **ana_admin** tenga acceso completo a todas las carpetas.

#### Configuración de Permisos

Primero, asignamos los permisos según el diseño:

1. **Permisos para `proyectos`**: Dar permisos completos a `javier_dev` y solo lectura a `maria_marketing`.

   ```bash
   sudo chown javier_dev /home/techsolutions/proyectos
   sudo chmod 770 /home/techsolutions/proyectos
   ```

   Esto permite que solo el propietario (`javier_dev`) y los usuarios del grupo puedan leer y escribir. Para asegurarnos de que **maria_marketing** tenga solo acceso de lectura, podemos crear un grupo específico para ello:

   ```bash
   sudo groupadd proyectos_lectura
   sudo usermod -aG proyectos_lectura maria_marketing
   sudo chown :proyectos_lectura /home/techsolutions/proyectos
   sudo chmod 750 /home/techsolutions/proyectos
   ```

2. **Permisos para `recursos`**: Dar acceso de solo lectura a `maria_marketing` y acceso completo a `ana_admin`.

   ```bash
   sudo groupadd recursos_completo
   sudo usermod -aG recursos_completo ana_admin
   sudo chown :recursos_completo /home/techsolutions/recursos
   sudo chmod 770 /home/techsolutions/recursos
   ```

3. **Permisos para `personal`**: `ana_admin` es la única con acceso completo, y el resto de usuarios no debería tener acceso.

   ```bash
   sudo chown ana_admin /home/techsolutions/personal
   sudo chmod 700 /home/techsolutions/personal
   ```

### Verificación de la Configuración de Permisos

Para verificar que los permisos están configurados correctamente, podemos usar el comando `ls -l`:

```bash
ls -l /home/techsolutions
```

Cada carpeta mostrará los permisos asignados y el propietario/grupo para asegurarnos de que solo los usuarios correctos tienen acceso.

### Resumen de Permisos

| Carpeta             | Usuario          | Permiso          |
|---------------------|------------------|------------------|
| proyectos           | javier_dev       | Lectura y escritura |
| proyectos           | maria_marketing  | Solo lectura     |
| recursos            | ana_admin        | Lectura y escritura |
| recursos            | maria_marketing  | Solo lectura     |
| personal            | ana_admin        | Lectura y escritura |
| personal            | Otros            | Sin acceso       |
