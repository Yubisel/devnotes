# Install .NET Framework 3.5 via DISM

Para instalar el .net framework desde el iso de windows montar la imagen en una torre virtual o introducir el disco en la unidad óptica, luego desde una terminal ejecutar el siguiente comando:

```cmd
Dism /online /enable-feature /featurename:NetFX3 /All /Source:%setupdrv%:\sources\sxs /LimitAccess
```

Sustituyendo ```%setupdrv%``` por la letra de la unidad donde esta montada la imagen o unidad óptica.

Otra forma de realizarlo es copiando el siguiente código en un fichero con la extención cmd y ejecutarlo desde una terminal. Este se encarga de buscar una a una por cada una de las unidades disponibles hasta que encuentra donde se encuentra 
la instalación de windows y comienza a instalar el .net framework.

```cmd
@echo off
Title .NET Framework 3.5 Offline Installer
for %%I in (D E F G H I J K L M N O P Q R S T U V W X Y Z) do if exist "%%I:\\sources\install.wim" set setupdrv=%%I
if defined setupdrv (
echo Found drive %setupdrv%
echo Installing .NET Framework 3.5...
Dism /online /enable-feature /featurename:NetFX3 /All /Source:%setupdrv%:\sources\sxs /LimitAccess
echo.
echo .NET Framework 3.5 should be installed
echo.
) else (
echo No installation media found!
echo Insert DVD or USB flash drive and run this file once again. 
echo.
)
pause
```
