## Anexo 2: Comandos Avanzados en `nano`

El editor `nano` ofrece una serie de comandos avanzados que mejoran la experiencia de edición. Estos comandos son útiles para navegar rápidamente dentro de un archivo, manipular el texto de forma precisa y realizar ediciones complejas sin necesidad de salir del entorno.

### Navegación Avanzada en `nano`

- **Ir al Inicio del Archivo**: `Ctrl + Y`
  - Mueve el cursor directamente al principio del archivo.

- **Ir al Final del Archivo**: `Ctrl + V`
  - Desplaza el cursor hasta el final del archivo.

- **Mostrar Números de Línea**: `Alt + #`
  - Activa o desactiva la numeración de líneas. Esto es útil para referencias rápidas al editar configuraciones o realizar cambios en líneas específicas.

- **Ir a una Línea Específica**: `Ctrl + _`
  - Abre una ventana en la que puedes ingresar el número de línea al que deseas moverte. Escribe el número y presiona `Enter` para ir directamente a esa línea.

### Búsqueda y Reemplazo de Palabras

- **Buscar Palabras o Frases**: `Ctrl + W`
  - Permite buscar un término específico en el archivo. Escribe el texto que buscas y presiona `Enter`. Para continuar buscando la misma palabra en el archivo, presiona `Ctrl + W` seguido de `Enter` nuevamente.

- **Reemplazar Palabras o Frases**: `Ctrl + \`
  - Abre el modo de reemplazo. Primero ingresa el término que deseas reemplazar, presiona `Enter`, luego ingresa el texto de reemplazo y presiona `Enter`. `nano` te pedirá confirmar cada reemplazo; puedes aceptar, rechazar o reemplazar todos.

### Manipulación de Texto en `nano`

#### Selección y Manipulación de Fragmentos

- **Seleccionar un Fragmento de Texto**: `Ctrl + Shift + 6` o `Alt + A`
  - Posiciona el cursor al inicio del texto que deseas seleccionar, y luego utiliza las flechas para expandir la selección. Este método permite seleccionar texto a nivel de caracteres, no solo de líneas.

- **Cortar el Fragmento Seleccionado**: `Ctrl + K`
  - Una vez que el texto está seleccionado, `Ctrl + K` lo corta. Este texto queda almacenado en el portapapeles y se puede pegar en otro lugar.

- **Pegar el Fragmento de Texto**: `Ctrl + U`
  - Pega el fragmento de texto en la posición actual del cursor.

#### Copiar y Manipular Múltiples Líneas

- **Seleccionar Múltiples Líneas**: `Alt + A`, luego usa las teclas de flecha
  - Presiona `Alt + A` para iniciar la selección, y utiliza las teclas de flecha para expandir la selección línea por línea.

- **Copiar las Líneas Seleccionadas**:
  - Primero, selecciona las líneas. Luego, en lugar de cortarlas con `Ctrl + K`, simplemente utiliza `Ctrl + U` en la nueva ubicación para pegarlas, manteniendo la selección en su lugar original.

- **Mover Líneas**:
  - Selecciona las líneas que deseas mover usando `Alt + A` y las teclas de flecha. Luego, usa `Ctrl + K` para cortar y `Ctrl + U` para pegar en la ubicación deseada.

#### Otras Funciones Útiles

- **Deshacer**: `Alt + U`
  - Deshace la última acción realizada en `nano`.

- **Rehacer**: `Alt + E`
  - Rehace la última acción deshecha, permitiéndote revertir un deshacer si fue accidental.

- **Moverse Entre Palabras**: `Ctrl + Espacio`
  - Desplaza el cursor palabra por palabra hacia adelante.

- **Activar o Desactivar Ajuste de Líneas (Wrap)**: `Alt + L`
  - Activa o desactiva el ajuste automático de líneas largas. Esto es útil cuando trabajas en archivos de texto largo y deseas ver el texto en una sola línea.

### Comandos para Configuraciones Adicionales

Algunos comandos adicionales permiten modificar el comportamiento de `nano` para ajustarlo a tus preferencias o necesidades específicas de edición:

- **Abrir en Modo Solo Lectura**: `nano -v nombre_del_archivo`
  - Este modo evita modificaciones accidentales en archivos críticos.

- **Forzar el Guardado de un Archivo con Permisos de Solo Lectura**: `Ctrl + O` seguido de `Ctrl + T`
  - Permite guardar un archivo aunque no tengas permisos de escritura. El sistema te pedirá autenticación de superusuario.

