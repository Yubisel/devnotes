# Permisos de apache sobre el sistema de ficheros

Esto es en modo desarrollo y si tiene 1 solo usuario por lo que no debería preocuparse por alguno más, tampoco por www-data.

Abrir el fichero de configuración de apache, el cual mayormente se ubica en:

```bash
/etc/apache2/apache2.conf
```

Buscar las líneas que definen el usuario y grupo que usa apache para trabajar

```bash
User ${APACHE_RUN_USER}
Group ${APACHE_RUN_GROUP}
```

y cambiar por

```bash
User username
Group usergroup
```

donde ```username``` y ```usergroup``` son los correspondietes al usuario admin
