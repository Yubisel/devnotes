# Rename several files at once, in batch, from console

Muchas veces tenemos un montón de ficheros y queremos modificar en lote su nombre de archivo por alguna circunstancia, añadir texto, suprimir texto, reemplazar algún carácter por otro... y nos llevaría mucho tiempo ir editando uno a uno cuando lo que queremos es hacer la misma edición para todos. Para ello podemos renombrar todos los archivos en lote, todos a la vez, usando desde una consola Linux el comando ***rename*** con expresiones regulares que nos permitan reemplazar lo que necesitemos. Mostraremos características, algunos ejemplos y la explicación de expresiones regulares para usar con ***rename***:

```bash
Usage:

    rename [ -h|-m|-V ] [ -v ] [ -n ] [ -f ] [ -e|-E *perlexpr*]*|*perlexpr* [ *files* ]
```

Please note that the rename command is part of the ***util-linux*** package and can be installed on a Debian or Ubuntu Linux, using the following syntax:
```bash
$ sudo apt-get install renameutils
```

### Características:

Parámetros del comando ***rename*** (poner a continuación del nombre del comando):

- ```-v``` (Verbose: modo detallado, nos saca en pantalla lo que se va haciendo en el proceso)
- ```-n``` (No Action: no realiza la acción, solo nos muestra lo que haría, es importante poner este parámetro junto con ```-v``` la primera vez que ejecutamos para comprobar que el resultado que vamos a obtener es el que queremos)
- ```-f``` (Force: sobrescribe los ficheros existentes)
- ```*.*``` para que trate todos los archivos con extensión del directorio
- ```\``` Localiza caracteres especiales tipo ```[ , {, (, -, _, ".", …```

### Ejemplos

1) Añadir algo tras una parte del nombre del archivo que es común en todos los ficheros. Ejemplo:

```imagenXXXX.jpg``` por ```imagen_NEW_XXXX.jpg```

```bash
# rename -v -n 's/imagen/imagen_NEW_/' *.jpg
```

2) Renombrar un grupo de imágenes con nombres diferentes. Si tenemos un grupo de imágenes (por ejemplo PNG) a las que les queremos añadir una información en la parte final del nombre de la imagen antes de la extensión. Ejemplo:
```nombreimagen.png``` por ```nombreimagen_150x150.png```

```bash
# rename -v -n 's/\.png/\_150x150.png/' *.png
```

3) Vamos a suponer que queremos reemplazar los guiones bajos por guiones medios ("_" por "-") en los nombres de nuestros archivos de un directorio determinado.

```bash
# rename -v -n 's/_/-/' *.jpg
```

Podemos ejecutar repetidamente por si hay varios guiones bajos en el nombre del archivo o utilizar expresiones regulares de repetición tipo {m} {n}...

4) Añadir texto al inicio del nombre del fichero. Con el carácter ^ le indicamos al comando rename que se sitúe en el comienzo del nombre del fichero y ahí inserte o ejecute la segunda parte. Ejemplo:

```
leccion 1.doc,
leccion 2.doc ...
```
por
```
tema - leccion 1.doc,
tema - leccion 2.doc ...
```

```bash
# rename -v -n 's/^/tema – /' *.doc
```

5) Si queremos eliminar varios caracteres antes de un punto de corte determinado. Ejemplo:
```
texto1_abc_001_small.jpg,
texto2_abc_002_small.jpg,
texto3_abc_003_small.jpg
```
por
```
texto1_small.jpg,
texto2_small.jpg,
texto3_small.jpg
```
Utilizamos para el corte la cadena ***"_small"*** y le decimos que nos elimine los 8 caracteres (\w) anteriores, o los reemplace por lo que indiquemos en la segunda parte del comando ***rename***.
```bash
# rename -v -n 's/\w{8}\_small/_small/' *.jpg
```

5b) Si queremos reemplazar desde un punto determinado de corte, pero respetando un número concreto de caracteres numéricos antes de la parte donde se produce el corte. Para este caso usamos el elemento "$1" en la cadena de la parte derecha, para que nos coja esa variable obtenida de la parte izquierda.  Viendo el ejemplo se entenderá mejor. Ejemplo:
```
texto1_uno001_small.jpg,
texto2_otro002_small.jpg,
texto3_cualquiera003_small.jpg ...
```
por
```
texto1_uno_ADD-001_small.jpg,
texto2_otro_ADD-002_small.jpg,
texto3_cualquiera_ADD-003_small.jpg ...
```
Utilizamos para el corte la cadena ***"_small"*** y le decimos que nos guarde los 3 caracteres numéricos (\d) anteriores (***001, 002, 003...***) utilizando el $1 en la segunda parte del comando (la expresión de la derecha) Nos añadirá o modificara lo indicado en la segunda parte del comando ***rename*** justo antes de esos 3 caracteres reservados antes del corte.
```bash
# rename -v -n 's/(\w{3})\_small/_ADD–$1_small/' *.jpg
```

6) Cambiar mayúsculas y minúsculas. Ejemplo:
```mi_fichero.txt``` por ```MI_FICHERO.TXT```
```bash
# rename -v -n 'y/a-z/A-Z/' *.txt
```

7) Eliminar del nombre del fichero caracteres especiales que no están entre la letra a y la z (a-z) . Dejando en el nombre del fichero solo caracteres alfanuméricos. Ejemplo:
```mi-fichero.txt``` por ```mifichero.txt```
```bash
# rename -v -n 'v/[^a-z]//' *.*
```

Para cambiar varios caracteres lo ejecutamos varias veces. Ejemplo:
```mi-fichero-con-varios-caracteres.txt``` por ```mificheroconvarioscaracteres.txt``` lo ejecutaremos 4 veces.

### Expresiones regulares

A continuación dejo un listado explicando (en inglés) las **expresiones regulares** que se pueden utilizar con este comando ***rename***:

- ```^``` matches the beginning of the line
- ```$``` matches the end of the line
- ```.``` Matches any single character
- ```(character)*``` match arbitrarily many occurences of (character)
- ```(character)?``` Match 0 or 1 instance of (character)
- ```[abcdef] ``` Match any character enclosed in [] (in this instance, a b c d e or f) ranges of characters such as [a-z] are permitted. The behaviour of this deserves more description.
- ```[^abcdef] ``` Match any character NOT enclosed in [] (in this instance, any character other than a b c d e or f)
- ```(character){m,n}``` Match m-n repetitions of (character)
- ```(character){m,}``` Match m or more repetitions of (character)
- ```(character){,n}``` Match n or less (possibly 0) repetitions of (character)
- ```(character){n}``` Match exactly n repetitions of (character)
- ```(expression)``` Group operator.
- ```expression1|expression2``` Matches expression1 or expression 2. Works with GNU sed, but this feature might not work with other forms of sed.
- ```\w``` matches any single character classified as a “word” character (alphanumeric or “_”)
- ```\W ``` matches any non-“word” character
- ```\s ``` matches any whitespace character (space, tab, newline)
- ```\S``` matches any non-whitespace character
- ```\d``` matches any digit character, equiv. to [0-9]
- ```\D ``` matches any non-digit character
