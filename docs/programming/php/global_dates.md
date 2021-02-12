# Mostrar fechas de acuerdo a uso horario

Problema de fecha utilizada en varios lugares del mundo (diferentes UTC). Se tiene como problema una aplicación
utilizada en diversas partes del mundo en donde se utiliza la misma fecha para mostrarse a diferentes usuarios
que pueden estar utilizando el APP en cualquier parte del mundo.

Para resolver este problema se utilizo el siguiente comando que se ejecuto en la base de datos:

```sql
SET time_zone = "+00: 00";
```

El mismo setea la zona horaria (```UTC-0```) por defecto en caso de que tu base de datos utilice ```timestamp```
automatizados para el update y creacion de datos.

Luego dentro del proyecto de servicios, por ejemplo ```Lumen``` con ```PHP```, necesitamos setear la siguiente linea en el
archivo de configuración ```.env```:

```yml
APP_TIMEZONE = UTC
```
