## Objetivos de la práctica

1. **Entender Como se Crea DockerFile**
2. **Ocultar la información sensible del código fuente**

## Descripción
Basándonos en lo que hemos hecho en la práctica 1, ahora vamos a crearnos un dockerfile pero vamos a ocultar la información sensible como el pass o el usario para que no es´te guardado en ningún fichero fuente.

Al nosotros eliminar el usuario y el passord metiéndolo en una variable de entorno ganamos:

- No podemos equivocarnos al desplegar en test o producción
- No podemos subir esa información al repositorio ( Fallo de seguridad )