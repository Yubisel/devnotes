# MySql

## Habilitar el acceso a mysql con el usuario root

Entrar en la terminal y conecctarse a mysql mediante el comando
```bash
sudo -u mysql
```

Luego ejecutar los siguientes comandos:

```sql
use mysql;

update user set plugin='mysql_native_password' where user='root';

flush privileges;
```

## Permitir en el phpmyadmin entrar sin password

```php
// Ubicacion del fichero en el sistema (Ubuntu)
/etc/phpyadmin/config.inc.php

$cfg['Servers'][$i]['AllowNoPassword'] = TRUE;
```
Se descomenta en las 2 líneas que aparece y luego para que los cambios tengan efecto se debe reiniciar el apache
