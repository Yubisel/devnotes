# Cómo encontrar una cadena o palabra específica en archivos y directorios

El siguiente comando mostrará una lista de todos los archivos que contienen una línea con el texto ```"check_root"```, buscando de forma recursiva y agresiva el directorio ```~/bin```.

```bash
grep -Rw ~/bin/ -e 'check_root'
```

Donde la opción ```-R``` le dice a ```grep``` que lea todos los archivos debajo de cada directorio, recursivamente, siguiendo los enlaces simbólicos solo si están en la línea de comando y la opción ```-w``` le indica que seleccione solo las líneas que contienen coincidencias que forman palabras completas, y ```-e``` se utiliza para especificar la cadena (patrón) a buscar.

Debe usar el comando sudo cuando busque en determinados directorios o archivos que requieren permisos de root (a menos que esté administrando su sistema con la cuenta de root).

```bash
sudo grep -Rw / -e 'check_root'	
```

Para ignorar las distinciones de casos, utilice la opción ```-i``` como se muestra:

```bash
grep -Riw ~/bin/ -e 'check_root'
```

Si desea saber la línea exacta donde existe la cadena de texto, incluya la opción ```-n```.

```bash
grep -Rinw ~/bin/ -e 'check_root'
```

Suponiendo que hay varios tipos de archivos en un directorio en el que desea buscar, también puede especificar el tipo de archivos que se buscarán, por su extensión, mediante la opción ```--include```.

Este ejemplo le indica a grep que solo mire todos los archivos ```.sh```.

```bash
grep -Rnw --include=\*.sh ~/bin/ -e 'check_root'
```

Además, es posible buscar más de un patrón, utilizando el siguiente comando.

```bash
grep -Rinw ~/bin/ -e 'check_root' -e 'netstat'
```
