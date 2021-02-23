Backup automático de una base de datos

Para tener los scripts ejecutables bien ordenados te recomiendo que los almacenes en «/usr/local/sbin«, así que accedemos a la ruta.
cd /usr/local/sbin

Creamos el script bash.
nano backup-db.sh

Copia y pega lo siguiente (con tus datos).
#!/bin/bash
#
# ---CONFIGURACIONES---
#
# Dato identificativo, por ejemplo la fecha YYYYMMDD.
DATE=$(date +"%Y%m%d")
# Directorio de backups, si no tienes debes crear uno.
# md /backup/mysql
BACKUP_DIR="/backup/mysql"
# Login y password de MySQL / MariaDB.
MYSQL_USER="root"
MYSQL_PASSWORD="TU-PASSWORD"
# Ejecutables de MySQL (normalmente no debes modificar estas rutas).
MYSQL=/usr/bin/mysql
MYSQLDUMP=/usr/bin/mysqldump
# Datos a omitir.
SKIPDATABASES="Database|information_schema|performance_schema|mysql"
# Numero de dias a conservar los backups (el resto se eliminaran automaticamente)
# Ejemplo 15 dias.
RETENTION=15
# FIN DE LA CONFIGURACION
# ---- EN LAS SIGUIENRES LINEAS NO DEBES MODIFICAR NADA ----
#
# Se crean las carpetas (por fecha) para los backups.
mkdir -p $BACKUP_DIR/$DATE
# Listar todas las bases de datos.
databases=`$MYSQL -u$MYSQL_USER -p$MYSQL_PASSWORD -e "SHOW DATABASES;" | grep -Ev "($SKIPDATABASES)"`
# Se generan los backups sql comprimidos con gzip.
for db in $databases; do
echo $db
$MYSQLDUMP --force --opt --user=$MYSQL_USER -p$MYSQL_PASSWORD --skip-lock-tables --events --databases $db | gzip > "$BACKUP_DIR/$DATE/$db.sql.gz"
done
# Borrar bakups antiguos (como definimos antes).
find $BACKUP_DIR/* -mtime +$RETENTION -delete

Guarda el script y cierra el editor.

Backup automático de una base de datos 2

 

Concedemos los permisos necesarios.
chmod 755 backup-db.sh

Creamos la tarea cron.
nano /etc/crontab

Insertamos una regla, por ejemplo queremos que se ejecute todos los días a las 3 am.
0 3 * * * root /usr/local/sbin/backup-db.sh

Guarda la tarea y cierra el editor.

Solo nos falta reiniciar el demonio cron, tienes dos opciones de comando dependiendo de tu distribución linux.
service cron restart

service crond restart

 

Ya tenemos nuestro script bash preparado para que a las tres de la madrugada nos genere nuestro primer bachup de la base de datos.