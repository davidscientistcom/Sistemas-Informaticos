### **Listas en Python**

Las **listas** son estructuras de datos en Python que se utilizan para almacenar varios elementos en una sola variable. Son como una colección de cosas que podemos usar para guardar números, cadenas de texto o incluso otras listas.

- Las listas se escriben entre **corchetes** `[]`.
- Pueden contener elementos de cualquier tipo (números, cadenas, booleanos, etc.).
- Los elementos de una lista tienen un índice que empieza en `0`.

#### **Características principales de las listas**
1. **Acceso por índice**: Podemos acceder a un elemento de la lista usando su posición.
2. **Modificables**: Puedes agregar, eliminar o cambiar elementos.
3. **Métodos útiles**: Python tiene muchas funciones integradas para trabajar con listas.



### **Ejemplos básicos para trabajar con listas**

#### 1. Crear una lista
```python
mi_lista = [1, 2, 3, 4, 5]
print(mi_lista)  # [1, 2, 3, 4, 5]
```

#### 2. Acceder a un elemento
```python
mi_lista = ["manzana", "plátano", "cereza"]
print(mi_lista[1])  # "plátano"
```

#### 3. Modificar un elemento
```python
mi_lista = [1, 2, 3]
mi_lista[1] = 10
print(mi_lista)  # [1, 10, 3]
```

#### **Ejemplo 1: Agregar elementos a una lista (`append`)**
```python
def agregar_elemento(lista, elemento):
    lista.append(elemento)
    return lista

print(agregar_elemento([1, 2, 3], 4))  # [1, 2, 3, 4]
```



#### **Ejemplo 2: Insertar elementos en una posición específica (`insert`)**
```python
def insertar_en_lista(lista, indice, elemento):
    lista.insert(indice, elemento)
    return lista

print(insertar_en_lista([1, 2, 3], 1, 10))  # [1, 10, 2, 3]
```



#### **Ejemplo 3: Eliminar el último elemento (`pop`)**
```python
def eliminar_ultimo(lista):
    lista.pop()
    return lista

print(eliminar_ultimo([1, 2, 3]))  # [1, 2]
```



#### **Ejemplo 4: Eliminar un elemento por su valor (`remove`)**
```python
def eliminar_elemento(lista, elemento):
    if elemento in lista:
        lista.remove(elemento)
    return lista

print(eliminar_elemento([1, 2, 3, 4], 3))  # [1, 2, 4]
```



#### **Ejemplo 5: Ordenar una lista (`sort`)**
```python
def ordenar_lista(lista):
    lista.sort()
    return lista

print(ordenar_lista([4, 2, 3, 1]))  # [1, 2, 3, 4]
```



#### **Ejemplo 6: Obtener el índice de un elemento (`index`)**
```python
def obtener_indice(lista, elemento):
    if elemento in lista:
        return lista.index(elemento)
    return -1

print(obtener_indice([1, 2, 3, 4], 3))  # 2
```



#### **Ejemplo 7: Contar cuántas veces aparece un elemento (`count`)**
```python
def contar_elementos(lista, elemento):
    return lista.count(elemento)

print(contar_elementos([1, 2, 2, 3, 2], 2))  # 3
```



#### **Ejemplo 8: Crear una copia de una lista (`copy`)**
```python
def copiar_lista(lista):
    return lista.copy()

original = [1, 2, 3]
copia = copiar_lista(original)
print(copia)  # [1, 2, 3]
```



#### **Ejemplo 9: Extender una lista con otra (`extend`)**
```python
def extender_lista(lista, elementos):
    lista.extend(elementos)
    return lista

print(extender_lista([1, 2, 3], [4, 5]))  # [1, 2, 3, 4, 5]
```



#### **Ejemplo 10: Invertir el orden de una lista (`reverse`)**
```python
def invertir_lista(lista):
    lista.reverse()
    return lista

print(invertir_lista([1, 2, 3, 4]))  # [4, 3, 2, 1]
```



### **Resumen de métodos de lista utilizados**
| **Método**     | **Descripción**                                |
|--|--|
| `append`       | Agrega un elemento al final de la lista.      |
| `insert`       | Inserta un elemento en una posición específica.|
| `pop`          | Elimina y devuelve el último elemento.        |
| `remove`       | Elimina la primera aparición de un elemento.  |
| `sort`         | Ordena la lista en orden ascendente.          |
| `index`        | Devuelve el índice de un elemento.            |
| `count`        | Cuenta cuántas veces aparece un elemento.     |
| `copy`         | Crea una copia de la lista.                  |
| `extend`       | Extiende la lista con otra lista o elementos. |
| `reverse`      | Invierte el orden de la lista.                |
