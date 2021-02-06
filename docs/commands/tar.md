# tar

Compres files

Usage: ```tar [OPTION...] [FILE]...```

- ```-c, --create``` create a new archive
- ```-u, --update``` only append files newer than copy in archive
- ```-x, --extract, --get``` extract files from an archive
- ```-f, --file=ARCHIVE``` use archive file or device ```ARCHIVE```
- ```-a, --auto-compress``` use archive suffix to determine the compression program
- ```-z, --gzip, --gunzip, --ungzip``` filter the archive through gzip
- ```-v, --verbose``` verbosely list files processed
- ```-t, --list``` list the contents of an archive

## Ejemplos

### Crear comprimido

```bash
tar -caf nombreFichero.tar.gz * # con el * comprime todo en el directorio donde se ejecuta el comando, sustituir por nombre de carpeta en case de querer solo una
```

### Mostrar el contenido de un fichero comprimido

```bash
tar -taf nombreFichero.tar.gz
```

### Extraer solo una de las carpetas o fichero dentro del comprimido

```bash
tar -xaf nombreFichero.tar.gz carpetaInterna
```
