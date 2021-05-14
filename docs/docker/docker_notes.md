# Docker

## Hacer cat a fichero dentro de contenedor

Esto es util para cuando hace falta el contenido de un fichero dentro del container por ejemplo luego de una instalacion de jenkins 

```bash
docker exec jenkins cat /var/jenkins_home/secrets/initialAdminPassword
```

----------------------------------------------------------------

Descargar una imagen oficial

```bash
-docker pull <nombreImagen>:<tag>
```

Mostrar contenedores corriendo (```-a``` los lista todos)

```bash
-docker ps
```

Mostrar la historia de las capas de una imagen ```--no-trunc``` no trunca la descripcion de salida

```bash
-docker history -H <nombreImagen> --no-trunc
```

Construir imagen de dockerfile ```tag``` no es requerido

```bash
-docker build -t <nombreimagen>:<tag> .
```

especificar un dockerfile con distinto nombre se suma la bandera ```-f <nombrefichero>```

Ver las imagenes que tienes en la pc

```bash
-docker images
```

Para filtrar imagenes dangling

```bash
-docker images -f dangling=true
```

Eliminar imagenes

```bash
-docker rmi (<id> || <nombreImagen>:<tag>)
```

Eliminar todas las imagenes dangling

```bash
-docker images -f dangling=true -q | xargs docker rmi
```

Correr container en base a una imagen

- ```-p <puertoMiMaquina>:<puertoContenedor>``` mapea el puerto del container al host
- ```-d``` correr el contenedor en segundo plano
- ```-t``` terminal
- ```-i``` interactive
- ```--name <nombreContainer>``` definir nombre al contenedor
- ```-e "<nombreVariableEntorno>=<valor>"```  definir variable de entorno
- ```-m | --memory "xxxmb" | "xxxgb"``` limitar la memoria que tendra como limite el contenedor
- ```-cpuset-cpus 0-3``` cantidad de cpu que podra usar el contenerdor ej: si son 2 sera 0-1
  
```bash
-docker run -d -p 80:80 <nombreImagen>:<tag> 
```

Borrar contenedor corriendo ```-f``` fuerza el borrado

```bash
-docker rm -fv <nombreContenedor>
```

Borrar todos los contenedores

```bash
-docker rm -fv $(docker ps -aq)
```

Detener un contenedor

```bash
-docker stop (<nombreContainer> || <idContainer>)
```

Iniciar contenedor

```bash
-docker start (<nombreContainer> || <idContainer>)
```

Reiniciar contenedor

```bash
-docker restart (<nombreContainer> || <idContainer>)
```

Cambiar nombre a un container corriendo

```bash
-docker rename <nombreViejo> <nombreNuevo>
```

Entrar a un container

- ```-t``` terminal
- ```-i``` interactivo
- ```-u root``` para entrar como otro usuario en este caso root

```bash
-docker exec -ti (<nombreContainer> || <idContainer>) bash
```

Ver detalles de contendor corriendo

```bash
-docker inspect <nombreContainer>
```

Saber cuantos recursos esta usando un contenedor

```bash
-docker stats <nombreContainer>
```

Copiar ficheros dentro de un contenedor

```bash
-docker cp fichero <container>:<ruta>
```

Copiar ficheros de un contenedor al host

```bash
-docker cp <container>:<ruta> <destino>
```

Para sobreescribir el CMD de una imagen se pasa el comando luego del nombre de la imagen al crear un contenerdor

```bash
-docker run -d -p 8080:8080 centos python -m SimpleHTTPServer 8080
```

Listas volumenes

```bash
-docker volume ls
```

Crear Volumen

```bash
-docker volume create <nombre>
```

Borrar volumen

```bash
-docker volume -rm <nombre>
```

Listar volumenes dangling

```bash
-docker volume ls -f dangling=true
```

Eliminar volumenes dangling

```bash
-docker volume ls -f dangling=true -q | xargs docker volume rm
```

Ejecutar comando desde fuera del container

```bash
-docker exec <nombreContainer> bash -c "<comando a ejecutar>"
```

### Redes

Listar las redes de docker

```bash
-docker network ls
```

Inspeccionar la red
(bridge) network por defecto de docker

```bash
-docker network inspect bridge
```

Crear red en docker

```bash
-docker network create <nombre>
```

Definir propiedades ej

```bash
-d # driver por defecto bridge
-subnet 172.142.10.0/24 
--gateway 172.124.10.1
```

Crear contenedor en una red distinta a la por defecto

```bash
-dcoker run --network <nombreRed> -d --name <nombreContenedor> <nombreImagen>
```

Conectar contenedores en distintas redes

```bash
-docker network conect <nombreRed> <nombreContenedorOtraRed>
```

Esto le poner al ```<nombreContenedorOtraRed>``` otra interfaz de red conectada a ```<nombreRed>```

Desconectar contenedor de una red

```bash
-docker network disconnect <nombreRed> <nombreContenedor>
```

Eliminar redes, si da error es porque tiene contenedores conectados

```bash
-docker network rm <nombreRed>
```

Asignar ip a contenedor

```bash
-docker run --network <nombreRed> --ip <ip>  -d --name <nombreContainer> <imagen>
```

### Salvar en fichero una imagen

Util cuando se quiere compartir una imagen sin depender de docker hub o un repositorio alojado en la nube, se comprime la imagen y se comparte como un fichero

```bash
docker save myusername/myproject:latest | gzip -c > myproject_img_bak20141103.tgz
```

Forma de cargarla en la pc destino 

```bash
gunzip -c myproject_img_bak20141103.tgz | docker load
```