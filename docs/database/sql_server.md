# SQL Server

## T-SQL: Listar todos los Procedimientos Almacenados

```sql
SELECT ROUTINE_NAME 
FROM INFORMATION_SCHEMA.ROUTINES 
WHERE ROUTINE_TYPE = 'PROCEDURE'
ORDER BY ROUTINE_NAME 
```

## Desencriptando Procedimientos almacenados

Para desencriptar procedimientos almacenados debemos hacer lo siguiente:

Si tenemos un procedimiento encriptado como el siguiente:

```sql
CREATE PROCEDURE desencriptame
WITH ENCRYPTION
AS
PRINT 'desencriptame porfavor'
GO
```

y si luego quiesieramos visualizar el texto de este procedimiento nos apareceria lo siguiente:
```sql
exec sp_helptext desencriptame

mensaje: Los comentarios de objeto han sido cifrados.
```
Para ver el código desencriptado hagamos lo siguiente. Vamos a crear un procedimiento que desencripte esto. este procedimiento se llamará ```DECRYPTSP2K``` y su código es el siguiente: compilenlo:

```sql
CREATE PROCEDURE [DBO].[DECRYPTSP2K](
@objName VARCHAR(50))
AS
DECLARE @a NVARCHAR(4000),
@b NVARCHAR(4000),
@c NVARCHAR(4000),
@d NVARCHAR(4000),
@i INT,
@t BIGINT
--get encrypted data
SET @a = (SELECT CTEXT
FROM SYSCOMMENTS
WHERE ID = OBJECT_ID(@objName))
SET @b = 'ALTER PROCEDURE ' + @objName + ' WITH ENCRYPTION AS ' +

REPLICATE('-',4000 - 62)

EXECUTE( @b)
--get encrypted bogus SP
SET @c = (SELECT CTEXT
FROM SYSCOMMENTS
WHERE ID = OBJECT_ID(@objName))
SET @b = 'CREATE PROCEDURE ' + @objName + ' WITH ENCRYPTION AS ' +

REPLICATE('-',4000 - 62)
--start counter
SET @i = 1

--fill temporary variable

SET @d = REPLICATE(N'A',(DATALENGTH(@a) / 2))

--loop

WHILE @i <= DATALENGTH(@a) / 2

BEGIN

--xor original+bogus+bogus encrypted

SET @d = STUFF(@d,@i,1,NCHAR(UNICODE(SUBSTRING(@a,@i,1)) ^ (UNICODE(SUBSTRING(@b,@i,1)) ^ UNICODE(SUBSTRING(@c,@i,1)))))

SET @i = @i + 1

END

--drop original SP

EXECUTE( 'drop PROCEDURE ' + @objName)

--remove encryption

--try to preserve case

SET @d = REPLACE((@d),'WITH ENCRYPTION','')

SET @d = REPLACE((@d),'With Encryption','')

SET @d = REPLACE((@d),'with encryption','')

IF CHARINDEX('WITH ENCRYPTION',UPPER(@d)) > 0

SET @d = REPLACE(UPPER(@d),'WITH ENCRYPTION','')

--replace SP

EXECUTE( @d)
```

Luego ejecutenlo de la siguiente manera:
```sql
exec DECRYPTSP2K 'desencriptame'
go
```

Luego vuelvan a ejecutar el sp_helptext y obtendrán el código desencriptado:
```sql
exec sp_helptext desencriptame
mensaje:
Text
CREATE PROCEDURE desencriptame
AS
PRINT 'desencriptame porfavor'
```
