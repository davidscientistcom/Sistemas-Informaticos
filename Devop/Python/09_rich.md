# Guía Detallada de Rich en Python

Rich es una librería de Python que permite crear interfaces de consola atractivas y funcionales. Con Rich, puedes mostrar tablas, textos estilizados, barras de progreso y más, todo ello con un enfoque simple y efectivo. En esta guía, exploraremos sus funcionalidades principales y aprenderemos a utilizarlas con ejemplos detallados.



## Introducción a Rich

Rich facilita la creación de interfaces de consola modernas con:

- Tablas y paneles.
- Colores y estilos en el texto.
- Barras de progreso y logs.
- Representación de datos como JSON o Markdown.

Instalación:
```bash
pip install rich
```

Importación básica:
```python
from rich.console import Console
console = Console()
```



## Principales Características de Rich

### 1. Texto Estilizado
Rich permite aplicar colores, negrita, cursiva y otros estilos al texto de la consola.

#### Ejemplo: Texto con estilos
```python
from rich.console import Console

console = Console()
console.print("[bold red]Error:[/] Ha ocurrido un problema.")
console.print("[italic green]Todo está funcionando correctamente.[/]")
console.print("[underline blue]Texto subrayado y azul.[/]")
console.print("[reverse yellow on black]Texto con colores inversos.[/]")
```

#### Principales etiquetas de estilo:
- `[bold]`: Negrita.
- `[italic]`: Cursiva.
- `[underline]`: Subrayado.
- `[reverse]`: Invierte los colores de texto y fondo.
- Colores: `[red]`, `[blue]`, `[green]`, etc.
- Fondos: `[on red]`, `[on green]`, `[on yellow]`, etc.
- Combinaciones: `[bold italic yellow on black]Texto combinado con fondo.[/]`.

### 2. Tablas
Las tablas permiten organizar datos de forma clara y legible.

#### Ejemplo: Crear una tabla simple
```python
from rich.console import Console
from rich.table import Table

console = Console()

table = Table(title="Usuarios")

table.add_column("ID", justify="right", style="cyan", no_wrap=True)
table.add_column("Nombre", style="magenta")
table.add_column("Edad", justify="right", style="green")
table.add_column("Rol", style="yellow")

table.add_row("1", "Ana", "25", "Admin")
table.add_row("2", "Luis", "30", "Usuario")
table.add_row("3", "María", "28", "Editor")

console.print(table)
```

#### Ejemplo: Celdas combinadas
```python
from rich.console import Console
from rich.table import Table

console = Console()

table = Table(title="Reporte Consolidado")

table.add_column("Categoría", style="cyan")
table.add_column("Detalles", style="magenta")

table.add_row("General", "Datos generales del sistema")
table.add_row("Memoria", "8 GB de RAM en uso")

table.add_row("[bold yellow]Resumen[/]", """[green]Sistema operativo: Linux
Procesadores: 8 cores
Disco: 500GB SSD[/green]""")

console.print(table)
```

#### Ejemplo: Bordes personalizados
```python
from rich.console import Console
from rich.table import Table

console = Console()

table = Table(title="Con estilo avanzado", border_style="bold magenta")

table.add_column("Nombre", style="cyan")
table.add_column("Valor", justify="right", style="green")

table.add_row("Temperatura", "25°C")
table.add_row("Humedad", "60%")
table.add_row("Presión", "1013 hPa")

console.print(table)
```

#### Ejemplo: Expandir tabla
```python
from rich.console import Console
from rich.table import Table

console = Console()

table = Table(title="Estadísticas", expand=True)

table.add_column("Métrica", style="cyan")
table.add_column("Valor", justify="right", style="green")

table.add_row("Velocidad", "100 Mbps")
table.add_row("Errores", "0")
table.add_row("Uptime", "99.99%")

console.print(table)
```

#### Configuraciones adicionales:
- `title`: Título de la tabla.
- `add_column()`: Define las columnas (nombre, estilo, alineación).
- `add_row()`: Agrega filas de datos.
- `border_style`: Personaliza el estilo del borde de la tabla.
- `expand`: Ajusta el tamaño de la tabla para llenar la consola.
- `padding`: Controla el espacio alrededor del texto en las celdas.
Las tablas permiten organizar datos de forma clara y legible.

#### Ejemplo: Crear una tabla
```python
from rich.console import Console
from rich.table import Table

console = Console()

table = Table(title="Usuarios")

table.add_column("ID", justify="right", style="cyan", no_wrap=True)
table.add_column("Nombre", style="magenta")
table.add_column("Edad", justify="right", style="green")
table.add_column("Rol", style="yellow")

table.add_row("1", "Ana", "25", "Admin")
table.add_row("2", "Luis", "30", "Usuario")
table.add_row("3", "María", "28", "Editor")

table.add_section()
table.add_row("4", "Pedro", "35", "Invitado")

console.print(table)
```

#### Configuraciones adicionales:
- `title`: Título de la tabla.
- `add_column()`: Define las columnas (nombre, estilo, alineación).
- `add_row()`: Agrega filas de datos.
- `add_section()`: Agrega una línea divisoria entre secciones de la tabla.
- `expand`: Ajusta el tamaño de la tabla para llenar la consola.

### 3. Barras de Progreso

#### Ejemplo: Barra de progreso
```python
from rich.console import Console
from rich.progress import Progress
import time

console = Console()

with Progress() as progress:
    tarea = progress.add_task("Cargando...", total=100)

    for i in range(100):
        time.sleep(0.05)
        progress.update(tarea, advance=1)
```

#### Ejemplo avanzado: Varias tareas con estilos personalizados
```python
from rich.progress import Progress, BarColumn, TextColumn, TimeRemainingColumn
import time

with Progress(
    TextColumn("[bold blue]{task.description}[/]"),
    BarColumn(),
    "[progress.percentage]{task.percentage:>3.0f}%",
    TimeRemainingColumn(),
    expand=True,
) as progress:
    tarea1 = progress.add_task("[cyan]Descargando archivo 1...", total=100)
    tarea2 = progress.add_task("[magenta]Descargando archivo 2...", total=200)

    while not progress.finished:
        progress.update(tarea1, advance=1)
        progress.update(tarea2, advance=2)
        time.sleep(0.05)
```

#### Configuraciones adicionales:
- `add_task(description, total)`: Define una tarea.
- `update(task_id, advance=n)`: Actualiza el progreso.
- `finished`: Verifica si todas las tareas han terminado.
- `TextColumn`: Define un texto descriptivo personalizado para las tareas.
- `BarColumn`: Personaliza la barra de progreso.
- `TimeRemainingColumn`: Muestra el tiempo estimado restante.
Las barras de progreso son útiles para mostrar el avance de tareas largas.

#### Ejemplo: Barra de progreso
```python
from rich.console import Console
from rich.progress import Progress
import time

console = Console()

with Progress() as progress:
    tarea = progress.add_task("Cargando...", total=100)

    for i in range(100):
        time.sleep(0.05)
        progress.update(tarea, advance=1)
```

#### Ejemplo avanzado: Varias tareas
```python
from rich.progress import Progress
import time

with Progress() as progress:
    tarea1 = progress.add_task("Descargando archivo 1...", total=100)
    tarea2 = progress.add_task("Descargando archivo 2...", total=200)

    while not progress.finished:
        progress.update(tarea1, advance=1)
        progress.update(tarea2, advance=2)
        time.sleep(0.05)
```

#### Configuraciones adicionales:
- `add_task(description, total)`: Define una tarea.
- `update(task_id, advance=n)`: Actualiza el progreso.
- `finished`: Verifica si todas las tareas han terminado.

### 4. Paneles
Los paneles son bloques visuales que destacan información.

#### Ejemplo: Crear un panel
```python
from rich.console import Console
from rich.panel import Panel

console = Console()

panel = Panel("Este es un mensaje importante", title="Aviso", subtitle="Información", border_style="blue")
console.print(panel)
```

#### Ejemplo avanzado: Panel anidado
```python
from rich.panel import Panel

inner_panel = Panel("Contenido interno", title="Interno", border_style="cyan")
outer_panel = Panel(inner_panel, title="Externo", border_style="magenta")

console.print(outer_panel)
```

#### Configuraciones adicionales:
- `border_style`: Define el color del borde.
- `expand`: Ajusta el tamaño del panel para llenar la consola.

### 5. Renderizado de JSON
Rich puede mostrar datos JSON de forma estructurada y con colores.

#### Ejemplo: Mostrar JSON
```python
from rich.console import Console

console = Console()

datos = {
    "nombre": "Ana",
    "edad": 25,
    "ciudad": "Madrid",
    "habilidades": ["Python", "DevOps"],
    "contacto": {
        "email": "ana@example.com",
        "telefono": "123456789"
    }
}

console.print_json(data=datos)
```

#### Configuraciones adicionales:
- `highlight`: Resalta el JSON con colores.
- `indent`: Ajusta la indentación del JSON.

### 6. Markdown
Puedes renderizar texto en formato Markdown en la consola.

#### Ejemplo: Renderizar Markdown
```python
from rich.console import Console
from rich.markdown import Markdown

console = Console()

md = Markdown("""# Título Principal

- Punto 1
- Punto 2

**Texto en negrita** y *texto en cursiva*.

```python
# Esto es código en Python
print("Hola, mundo!")
```
""")

console.print(md)
```

#### Ejemplo: Markdown con tablas
```python
from rich.console import Console
from rich.markdown import Markdown

console = Console()

md = Markdown("""# Tabla de Ejemplo

| Columna 1 | Columna 2 | Columna 3 |
|--|--|--|
| Valor 1   | Valor 2   | Valor 3   |
| Valor 4   | Valor 5   | Valor 6   |

**Nota**: Las tablas permiten estructurar información de forma clara.
""")

console.print(md)
```

#### Ejemplo: Markdown avanzado con listas y código
```python
from rich.console import Console
from rich.markdown import Markdown

console = Console()

md = Markdown("""# Características Avanzadas

1. **Listas numeradas**:
   - Subpunto 1
   - Subpunto 2

2. **Código embebido**:
   ```bash
   echo "Hola, mundo!"
   ```

3. **Tablas**:
   | Atributo    | Valor    |
   |-|-|
   | Velocidad   | 100 Mbps |
   | Errores     | 0        |

""")

console.print(md)
```

#### Configuraciones adicionales:
- `code blocks`: Renderiza bloques de código con colores.
- Compatibilidad total con sintaxis de Markdown para crear encabezados, listas, tablas y más.
Puedes renderizar texto en formato Markdown en la consola.

#### Ejemplo: Renderizar Markdown
```python
from rich.console import Console
from rich.markdown import Markdown

console = Console()

md = Markdown("""# Título Principal

- Punto 1
- Punto 2

**Texto en negrita** y *texto en cursiva*.

```python
# Esto es código en Python
print("Hola, mundo!")
```
""")

console.print(md)
```

#### Configuraciones adicionales:
- `code blocks`: Renderiza bloques de código con colores.

### 7. Logging
Rich mejora el sistema de logging de Python con colores y estilos.

#### Ejemplo: Logs con Rich
```python
import logging
from rich.logging import RichHandler

logging.basicConfig(
    level=logging.INFO,
    format="%(message)s",
    handlers=[RichHandler()]
)

log = logging.getLogger("rich")
log.info("Este es un mensaje informativo.")
log.warning("Este es un mensaje de advertencia.")
log.error("Ha ocurrido un error.")
```



## Proyecto Completo: Monitor de Sistema para Linux
Este programa utiliza Rich para mostrar información detallada sobre el sistema, como uso de CPU, memoria, y espacio en disco.

### Código del Programa
```python
import os
import psutil
from rich.console import Console
from rich.table import Table
from rich.panel import Panel
from rich.progress import Progress

def mostrar_uso_cpu(console):
    cpu_uso = psutil.cpu_percent(interval=1)
    console.print(Panel(f"Uso de CPU: [bold green]{cpu_uso}%[/]", title="CPU"))

def mostrar_memoria(console):
    memoria = psutil.virtual_memory()
    total = round(memoria.total / (1024 ** 3), 2)
    usado = round(memoria.used / (1024 ** 3), 2)
    disponible = round(memoria.available / (1024 ** 3), 2)
    console.print(Panel(f"Total: {total} GB\nUsado: {usado} GB\nDisponible: {disponible} GB", title="Memoria RAM"))

def mostrar_espacio_disco(console):
    tabla = Table(title="Espacio en Disco")
    tabla.add_column("Dispositivo", style="cyan")
    tabla.add_column("Total", justify="right")
    tabla.add_column("Usado", justify="right")
    tabla.add_column("Libre", justify="right")

    for particion in psutil.disk_partitions():
        uso = psutil.disk_usage(particion.mountpoint)
        tabla.add_row(
            particion.device,
            f"{round(uso.total / (1024 ** 3), 2)} GB",
            f"{round(uso.used / (1024 ** 3), 2)} GB",
            f"{round(uso.free / (1024 ** 3), 2)} GB"
        )

    console.print(tabla)

def main():
    console = Console()

    with Progress() as progress:
        tarea = progress.add_task("Recopilando información del sistema...", total=3)

        mostrar_uso_cpu(console)
        progress.update(tarea, advance=1)

        mostrar_memoria(console)
        progress.update(tarea, advance=1)

        mostrar_espacio_disco(console)
        progress.update(tarea, advance=1)

if __name__ == "__main__":
    main()
```