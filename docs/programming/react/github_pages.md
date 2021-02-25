# Deploy react app to github pages

## Prerequisitos

- Cuenta en Github
- Git instalado y configurado en la pc
- Node y npm (min v8.10)
- Tener instalado create-react-app para crear aplicación react

## Procedimiento

- Crear un repositorio en Github, para que funcione la página si no es una cuenta de pago el repositorio debe ser público. 
- Crear aplicación react ```npx create-react-app app-name```, al hacer esto el comando inicializará además en el proyecto el repositorio git
- Luego instalar el módulo github-pages como dependencia de desarollo
```bash
cd app-name
npm install gh-pages --save-dev
```
- Agregar la propiedad ```homepage``` en la parte superior del fichero ```package.json``` como string con el valor ```https://{username}.github.io/{repo-name}``` donde el ```{username}``` es su nombre de usuario en Github y el ```{repo-name}``` el nombre del repositorio, quedando de la siguiente forma: 
```bash
"homepage": "https://{username}.github.io/{repo-name}"
```
- En la sección de ```scripts``` necesitamos agregar 2 nuevos
```json
"scripts": {
    ...
    "predeploy": "npm run build",
    "deploy": "gh-pages -d build"
  },
```
- Agregar el repositorio remoto al proyecto
```bash
git remote add origin git@github.com:{username}/{repo-name}.git
```
- Ahora a desplegar a las páginas de GitHub ejecutando el siguiente comando
```bash
npm run deploy
```
Este comando creará una rama con el nombre ```gh-pages``` en github que tendrá hosteada la app en la url definida en la propiedad ```homepage``` definida en el ```package.json```
- Haga commit y actualice la rama master de su repositorio remoto (opcional)
```bash
git add .
git commit -m "Your awesome message"
git push origin master
```

Nota: En caso de no visualizar el sitio en la url ```https://{username}.github.io/{repo-name}``` podrías chequear en la configuración del repositorio ```Settings``` sección ```GitHub Pages``` y seleccionar la rama ```gh-pages``` y ```/root``` para redirigir correctamente al proyecto compilado.
