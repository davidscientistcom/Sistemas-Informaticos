
### Obtener una lista de recursos

El verbo HTTP GET se utiliza para obtener una lista de recursos. Por ejemplo, para obtener una lista de todos los usuarios de un sistema, la URL sería la siguiente:

```
https://api.example.com/users
```

### Obtener un recurso específico
Para obtener un recurso específico, se puede utilizar el verbo HTTP GET con el identificador del recurso como parte de la URL. Por ejemplo, para obtener el usuario con el identificador 12345, la URL sería la siguiente:

```
https://api.example.com/users/12345
```
### Crear un recurso:

El verbo HTTP POST se utiliza para crear un recurso nuevo. La URL sería la misma que para obtener una lista de recursos, pero el cuerpo de la solicitud HTTP contendría los datos del nuevo recurso. Por ejemplo, para crear un nuevo usuario, la solicitud HTTP podría ser la siguiente:

```
POST /users
Content-Type: application/json

{
  "name": "John Doe",
  "email": "johndoe@example.com"
}

```

### Eliminar un recurso

El verbo HTTP DELETE se utiliza para eliminar un recurso existente. La URL sería la misma que para obtener un recurso específico. Por ejemplo, para eliminar el usuario con el identificador 12345, la URL sería la siguiente:

```
DELETE /users/12345
```


### Ejemplos:

```Python
# Obtener una página web
https://www.example.com

#Obtener el contenido de un archivo
https://api.example.com/files/my-file.txt

#Realizar una búsqueda
https://api.example.com/search?q=keyword

#Realizar una transacción
https://api.example.com/transactions
```