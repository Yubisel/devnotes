# git

## Mostrar grafica de commits en consola

```bash
git log --branches --remotes --tags --graph --oneline --decorate
```

## Cambios locales que no se han subido al server

```bash
git log --branches --not --remotes=origin
```

por revisar

```bash
git log --reverse --pretty=format:'"%h":{%n "parents":"%p",%n "message":"%s",%n "dat":"%cr",%n "name":"%an"%n},'
```


## Exportar de una revisión a otra en git

Este comando es sobre el cmd de windows

```cmd
for /f "usebackq tokens=*" %A in (`git diff-tree -r --no-commit-id --name-only --diff-filter=ACMRT 441dc870f52ee04068a5aba6a2c4acd1d6840ffe HEAD`) do echo FA|xcopy "%~fA" "C:\git_changed_files\%A"
```

## Guardar credenciales

Se usa para que no las solicite cada vez que se realiza una operación sobre los remotes, tener cuidado que se almacenan en texto plano en ```/home/username/.git-credentials```

```bash
git config --global credential.helper store
```

## Definir usuario y email para los commits 

Esta configuración se alamcena en ```/home/username/.gitconfig```

```bash
git config --global user.email "usuario@servidor.com"
git config --global user.name "Nombre Usuario"
```

## Cambiar origen de repositorio de https a ssh

Para ver donde apuntan los origenes del repositorio se ejecuta el comando ```git remote -v``` el que tiene como salida algo como esto:

```bash
origin	https://usuario@servidor.org/url/del/repositorio.git (fetch)
origin	https://usuario@servidor.org/url/del/repositorio.git (push)
```

Para cambiar la url de origen a ```ssh``` se realiza con el siguiente comando:

```bash
git remote set-url origin git@servidor.org:url/del/repositorio.git
```

Si volvieramos a ejecutar ```git remote -v``` veremos como nuestro origen a cambiado de ```https``` a ```ssh```

```bash
origin	git@servidor.org:url/del/repositorio.git (fetch)
origin	git@servidor.org:url/del/repositorio.git (push)
```

Nota: en caso de querer cambiar de ```ssh``` a ```https``` sería el mismo proceso pero cambiando la url
