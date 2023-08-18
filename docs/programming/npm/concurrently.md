# Concurrently

Run multiple commands concurrently. Like npm run watch-js & npm run watch-less but better.
[docs](https://www.npmjs.com/package/concurrently)

## Installation


concurrently can be installed in the global scope (if you'd like to have it available and use it on the whole system) or locally for a specific package (for example if you'd like to use it in the scripts section of your package):


|	     |	npm  |	Yarn |	pnpm |	Bun |
|------|-------|-------|-------|------|
|Global | 	npm i -g concurrently |	yarn global add concurrently |	pnpm add -g concurrently |	bun add -g concurrently|
|Local* |	npm i -D concurrently |	yarn add -D concurrently |	pnpm add -D concurrently |	bun add -d concurrently|

*It's recommended to add concurrently to devDependencies as it's usually used for developing purposes. Please adjust the command if this doesn't apply in your case.




## How to use

- Initialize a npm project `npm init --y` for use through `npm run` and less verbosability with commands
- Install the package concurrently
- Create scripts in `package.json` to start each project individually see example below
- Create general script with concurrently to execute all the previous at the same time
- Run your project



### Example

With this folder structure

- source
  - admin (react admin application created with `create-react-app`)
  - app (next application to show some specific data created with `create-next-app`)
  - api (nest backend api)

* Each folder have it's own .env file with it's own values, but for example the admin application need to define excecution port to avoid error and becouse react and next use the same port (3000) by default 

In `admin/.env` file define `PORT=3001` and react will use this one instead of default one.


- Execute `npm init --y` this create the `package.json` file

```json
{
  "name": "source",
  "version": "1.0.0",
  "description": "",
  "main": "index.js",
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1"
  },
  "keywords": [],
  "author": "",
  "license": "ISC"
}
```

- Install concurrently dependency  `npm install -D concurrently`
- Modify `scripts` section as follow

```json
  "start:api": "npm --prefix ./api/ run start:dev",
  "start:app": "npm --prefix ./app/ run dev",
  "start:admin": "npm --prefix ./admin/ run start"
```

`"start:api"` start the nest api

`"start:app"` start the next app

`"start:admin"` start the react app

- Add the new one to run all with concurrently

`"dev": "concurrently -c \"npm run start:api\" \"npm run start:app\" \"npm run start:admin\""`

With which we can run all environments at the same time, remmember that each one has to be wrapped in quotes. In this way, the log of each one identified with numbers `[0]`, `[1]` and `[2]`. To assign a name to each one you can use the `--name` parameter, for example: `--names \"API,APP,ADMIN\"`. This will help visually to identify logs.

There is some other options for colorize or use shorthands read the [docs](https://www.npmjs.com/package/concurrently) for know more about it.

In my case this was the final `package.json`

```json
{
  "name": "source",
  "version": "1.0.0",
  "description": "",
  "main": "index.js",
  "scripts": {
    "start:api": "npm --prefix ./api/ run start:dev",
    "start:app": "npm --prefix ./app/ run dev",
    "start:admin": "npm --prefix ./admin/ run start",
    "dev": "concurrently -c \"blue.bold,magenta.bold,yellow.bold\" \"npm run start:*\""
  },
  "keywords": [],
  "author": "",
  "license": "ISC",
  "devDependencies": {
    "concurrently": "^8.2.0"
  }
}
```