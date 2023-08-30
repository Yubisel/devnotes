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

## Compare changes from branch XXX against master

```shell
git reset $(git merge-base master XXX)
```

## Create a fake commit and trigger the new build

```shell
git commit -a --amend --no-edit --no-verify; git push --force-with-lease
```

## Create an empty commit
```shell
git commit --allow-empty -m "My empty commit with a message"
```

## Rename git branch locally and remotely

```shell
git branch -m old_branch new_branch         # Rename branch locally    
git push origin :old_branch                 # Delete the old branch    
git push --set-upstream origin new_branch   # Push the new branch, set local branch to track the new remote
```

### Takes a patch (e.g. the output of git diff ) and applies it to the working directory

```shell
git apply << EOF
... some diff here
EOF
```

### Attaches a pull request for the current branch to the existing issue number x using hub

```shell
hub pull-request -i x
```

## Remove all your local git branches but keep master

```shell
git branch | grep -v "master" | xargs git branch -D
```

## Pull multiple projects automatically

```shell
find . -mindepth 1 -maxdepth 1 -type d -print -exec git -C {} pull \;
```

## Remove all files which are not tracked by git

```shell
git clean -fxd
```

## Move branches around 

Reassign a branch to a commit with the -f option. It moves (by force) the main branch to three parents behind HEAD.

```shell
git branch -f main HEAD~3
```

## Shorthand for a fetch and a rebase

```shell
git pull --rebase
```

## Show any action performed in git

```shell
git reflog
```

## Print the SHA1 hashes given a revision

```shell
git rev-parse
```

## Remove all branches matching a pattern

```shell
git branch | grep "<pattern>" | xargs git branch -D
```

## Check all differences between two branches and apply to a new branch

```shell
git checkout master
git diff master..your-branch > mypatch.patch
git checkout -b new-branch
git apply mypatch.patch
```

## Remove all untracked files

To see which files will be deleted:

```shell
git clean -n
```

To remove all files:
```shell
git clean -f
```

## Revert latest commit which was pushed

Create a new commit that undoes the changes of a previous commit.

```shell
git revert HEAD
```

## Create a path file

```shell
git diff > my_patch.patch
```

## Apply a path file

```shell
 git apply my_patch.patch
```