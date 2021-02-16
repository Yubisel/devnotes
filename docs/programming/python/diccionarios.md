# Diccionarios

## Crear

```python
diccionario = {
    "nombre": "Jonh",
    "sexo": "Masculino",
    "edad": 35
}
```

Además se puede crear usando el constructor dict

```python
persona = dict(nombre="Jonh Doe", sexo="Masculino")
```

## Acceder a un elemento

```python
diccionario["nombre"]
# o
diccionario.get('nombre')
```

## Modificando valores

```python
diccionario["nombre"] = "Myke"
```

## Longitud (len)

```python
len(diccionario)
```

## Agregar valores

```python
diccionario["direccion"] = "Time scuare 1256"
```

## Eliminar obteniendo el valor (pop)

```python
edad = diccionario.pop('edad')
```

## Eliminar último valor agregado (popitem)

```python
diccionario.popitem()
```

Remueve el último elemento que fue insertado en el diccionario. El valor de retorno es el item eliminado en forma de tupla.

## Eliminar (del)

```python
del diccionario["edad"]
```

## Crear copia (copy)(dict)

```python
diccionario2 = diccionario.copy()
```

Otra forma de hacerlo es usando ```dict```

```python
diccionario2 = dict(diccionario)
```

## Eliminar todos los elementos (clear)

```python
diccionario.clear()
```
