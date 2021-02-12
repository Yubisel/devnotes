# CDPATH

CDPATH es una variable de entorno. Muy parecida a PATH que contiene muchas rutas diferentes concatenadas usando ':'

## Uso

Digamos, por ejemplo, que el usuario accede con frecuencia a algunos directorios presentes en un directorio ```"X"```.
Cada vez que el usuario quiere llegar a cualquiera de estos directorios presentes en ```"X"```, la mayoría de las veces
lo atraviesa dando la ruta absoluta, lo que lleva poco tiempo si tiene que encontrarla. Qué bueno sería si pudiéramos
hacer ```"cd"``` en el directorio en particular como si ese directorio que está buscando estuviera justo debajo de su
directorio actual. Esto es lo que logra ```CDPATH```.

Normalmente, cuando se da el comando ```cd```, verifica el nombre del directorio en el directorio actual y arroja un error
si no se encuentra, de lo contrario, atraviesa el directorio. Si se establece el ```CDPATH```, el comando ```"cd"```
comienza a buscar el directorio en la lista de directorios presentes en la variable ```CDPATH``` y luego hace que el
directorio cambie de manera apropiada.

## Como definir la varialbe CDPATH

Definir la variable ```CDPATH``` es exactamente igual que con la variable ```PATH```, puedes agregar mas de un directorio
de entrada separando las rutas por ```:```, por ejemplo:

```bash
$ export CDPATH=".:/home/guru:/usr"
```

Para que tengas estos cambios cada vez que inicies sesión deberias ubicar la exportación dentro de el fichero ```~/.bash_profile``` o ```~/.profile```
