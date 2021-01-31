# Habilitar selección de la versión de windows en el proceso de instalación

Añadir un archivo llamado ```"ei.cfg"``` a la iso o USB creado con la herramienta de creación de medios para que me permita elegir al inicio de la instalación que versión quiero instalar.

El archivo en concreto se llama ```"ei.cfg"``` y hay que copiarlo dentro de la carpeta ```"Source"``` que hay en la iso o USB creados con la herramienta de creación de medios.

Este archivo se puede crear fácilmente. Simplemente hay que abrir un bloc de notas y copiar lo siguiente:

```cmd
[Channel]
Retail
```

Una vez copiado eso hay que guardar el archivo con el nombre ```"ei.cfg"``` fijándose que se queda con extensión CFG y que no se llama ```"ei.cfg.txt"```

Una vez se tiene creado este archivo ```"ei.cfg"``` se copia en la carpeta source como decía antes y cuando se inicie la instalación limpia de Windows 10 te preguntará que versión quieres instalar.
