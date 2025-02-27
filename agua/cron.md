## Programación de Tareas con Cron en Ubuntu

### 1. ¿Qué es Cron?

**Cron** es una utilidad en sistemas Unix/Linux que permite programar la ejecución automática de comandos o scripts a intervalos regulares o en momentos específicos. Esto es muy útil para tareas de mantenimiento, copias de seguridad, actualizaciones periódicas o cualquier otra acción que se quiera automatizar.

El proceso que ejecuta las tareas programadas se llama **daemon cron** y se inicia automáticamente en el arranque del sistema.



### 2. El Archivo Crontab

El **crontab** es un archivo de configuración donde se definen los comandos a ejecutar y el momento en que deben ejecutarse. Cada usuario puede tener su propio crontab, y existe uno global para tareas del sistema.

Para trabajar con el crontab de tu usuario, se utilizan los siguientes comandos:

- **Editar el crontab:**  
  ```bash
  crontab -e
  ```  
  Este comando abre el editor de texto configurado (por defecto puede ser *nano*, *vim*, etc.) para que agregues, modifiques o elimines las tareas.

- **Listar las tareas programadas:**  
  ```bash
  crontab -l
  ```

- **Eliminar el crontab:**  
  ```bash
  crontab -r
  ```



### 3. Sintaxis de una Entrada en el Crontab

Cada línea en el crontab representa una tarea programada (o *cron job*) y tiene la siguiente estructura:

```
* * * * * comando
│ │ │ │ │
│ │ │ │ └─ Día de la semana (0-7) [0 y 7 representan el domingo]
│ │ │ └─── Mes (1-12)
│ │ └───── Día del mes (1-31)
│ └─────── Hora (0-23)
└───────── Minuto (0-59)
```

**Ejemplo:**  
Para ejecutar un script de Python ubicado en `/home/usuario/script.py` todos los días a las 8:30 AM, la entrada sería:

```
30 8 * * * /usr/bin/python3 /home/usuario/script.py
```

Aquí:
- `30`: El minuto 30.
- `8`: La hora 8 (8:00 AM).
- `* * *`: Se ejecuta todos los días del mes, todos los meses y todos los días de la semana.



### 4. Caracteres Especiales y Atajos

Además de la sintaxis numérica, **cron** acepta algunas palabras clave que facilitan la programación:

- `@reboot`: Ejecuta el comando una sola vez al reiniciar el sistema.  
  **Ejemplo:**  
  ```
  @reboot /home/usuario/inicializa.sh
  ```

- `@daily` o `@midnight`: Ejecuta el comando una vez al día, generalmente a medianoche.  
  **Ejemplo:**  
  ```
  @daily /home/usuario/backup.sh
  ```

- `@weekly`: Ejecuta el comando una vez a la semana.  
- `@monthly`: Ejecuta el comando una vez al mes.  
- `@yearly` o `@annually`: Ejecuta el comando una vez al año.



### 5. Ejemplos Prácticos

#### Ejemplo 1: Ejecutar un script cada 15 minutos

Si deseas ejecutar un script ubicado en `/home/usuario/monitor.sh` cada 15 minutos, puedes usar:

```
*/15 * * * * /home/usuario/monitor.sh
```

La expresión `*/15` en el campo de los minutos indica "cada 15 minutos".

#### Ejemplo 2: Realizar una copia de seguridad diaria

Supongamos que quieres realizar una copia de seguridad de una carpeta todos los días a las 2:00 AM. Puedes agregar la siguiente línea en tu crontab:

```
0 2 * * * /home/usuario/backup.sh > /home/usuario/backup.log 2>&1
```

Aquí:
- `0 2 * * *`: Ejecuta a las 2:00 AM exactamente.
- El comando redirige la salida estándar y de errores a un archivo de log para poder revisar el resultado de la copia.

#### Ejemplo 3: Reiniciar un servicio al iniciar el sistema

Para reiniciar un servicio (por ejemplo, Apache) cada vez que el sistema se reinicie, puedes usar el atajo `@reboot`:

```
@reboot sudo systemctl restart apache2
```

> **Nota:** Al usar `sudo` en un cron job, asegúrate de que el usuario tenga los permisos adecuados o que se configure el sudoers sin requerir contraseña para ese comando.



### 6. Buenas Prácticas

- **Redirección de Salida:**  
  Es recomendable redirigir la salida de los comandos a un archivo de log para poder depurar errores.  
  Ejemplo:  
  ```bash
  0 3 * * * /home/usuario/script.sh >> /home/usuario/script.log 2>&1
  ```

- **Variables de Entorno:**  
  Ten en cuenta que el entorno de ejecución de un cron job es diferente al de una sesión interactiva. Si tu script depende de ciertas variables de entorno, es aconsejable definirlas explícitamente en el crontab o dentro del script.

- **Rutas Absolutas:**  
  Siempre utiliza rutas absolutas en los comandos y scripts, ya que el cron no siempre ejecuta el comando desde el directorio esperado.

- **Comprobación de Permisos:**  
  Verifica que el usuario que ejecuta el cron tenga los permisos necesarios para ejecutar los comandos y acceder a los archivos involucrados.


