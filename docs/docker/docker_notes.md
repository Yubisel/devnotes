
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

## Redes

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

## Salvar en fichero una imagen

Util cuando se quiere compartir una imagen sin depender de docker hub o un repositorio alojado en la nube, se comprime la imagen y se comparte como un fichero

```bash
docker save myusername/myproject:latest | gzip -c > myproject_img_bak20141103.tgz
```

Forma de cargarla en la pc destino 

```bash
gunzip -c myproject_img_bak20141103.tgz | docker load
```

## Inspect a Docker Image's Content Without Starting a Container

Docker images can bundle arbitrary binaries and libraries into a single blob of data. Inspecting what's actually inside an image helps you assess its suitability and identify any security hazards.

The easiest way to explore an image's content involves starting a container, getting a shell session, and then using regular terminal commands like `ls` and `cd` to view its directory structure from within. This isn't ideal in security-critical environments though - creating a container with an unknown image could expose you to a malicious entrypoint script.

Here are techniques you can use to inspect an image's files without starting a container.

### Creating a Container Without Starting It 

 
`docker create` is a lesser-known counterpart to docker run. It creates a new container atop a given image without starting it. You could launch it later on with the docker start command.

Creating a new container isn't dangerous as it'll stay inert until it's run. You can roughly liken it to defining the config settings for a VM which you don't use. Even if it's set to boot from a tainted operating system ISO, you're not going to cause any damage to your environment.

`docker create --name suspect-container suspect-image:latest`

The command above creates a new container called suspect-container that will be based on the suspect-image:latest image.

###  Exporting the Container's Filesystem

Now you've got a valid but stopped container, you can export its filesystem using the docker export command. As the container's never been started, you can be sure the export accurately represents the filesystem defined by your image's layers.

`docker export suspect-container > suspect-container.tar`

You'll end up with a tar archive in your working directory that contains everything inside your image. Open or extract this archive using your favorite software to browse the image's directories and list and view files.

If you don't need to save or open the archive, instead preferring to get the file list in your terminal, modify the tar command:

`docker export suspect-container | tar t > suspect-container-files.txt`

`tar t` lists the contents of the input archive. You'll end up with a list of everything in your image inside `suspect-container-files.txt`.


###  Using "docker image save" 

A variation on this technique is using docker image save. This command directly saves an image's data to a tar archive.

`docker image save suspect-image:latest > suspect-image.tar`

This method produces an archive that's focused on the image, not containers created from it. The tar will include a manifest.json file, describing the image's layers, and a set of directories containing the content of all the individual layers.

This is helpful when you're evaluating each layer's role in building the image. However, creating and exporting a stopped container is a more accessible way to browse the image's final filesystem.

### Listing Layers With "docker image history"

Another way of inspecting an image's content is to view its layer list with the docker image history command.

`docker image history suspect-image:latest`

This exposes the Dockerfile instructions that composed the image's layers. It won't let you see individual files and directories in the image's filesystem but can more be effective at highlighting suspect behavior.

Each line in the command's output represents a new layer in the image. The "CREATED BY" column shows the Dockerfile instruction that created the layer.

Scanning the layer list helps you quickly identify suspicious actions that could indicate you're using a malicious image. Look for unknown binaries in RUN instructions, unexpected environment variable changes, and suspicious CMD and ENTRYPOINT statements.

The latter two layers are arguably the most important to assess when inspecting an image's history. They tell you exactly what will launch when you docker run or docker start a container. If either instruction looks suspicious or unfamiliar, consider using the techniques above to fully inspect the referenced binaries or scripts.

Accessing an image's filesystem provides a very granular view of its contents where malicious content can easily go unnoticed, even after manual inspection. The layer list exposed by docker image history can't help you find disguised filesystem items but is more effective at surfacing blatantly malicious operations such as furtive spyware downloads or environment variable overrides.