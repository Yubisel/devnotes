# ssh

## Create ssh key

Las herramientas para crear y usar SSH son estándar y están presentes en la mayoría de las distribuciones de Linux. Con los siguientes comandos, puede generar la key ssh.

- Corre: ```ssh-keygen -t rsa```. Para una llave más segura de 4096-bit key, corra: ```ssh-keygen -t rsa -b 4096```
- Al dar enter se preguntará donde quiere guardar la clave que se va a generar, en caso de dar enter guardará la key en el lugar por defecto ```/home/username/.ssh/id_rsa```
- Luego se deberá introducir una frase y su confirmación.
- Como resultado se informa el lugar donde se guardó la llave publica y privada además del fingerprint.

**Nota**: en caso de especificar el parámetro ```-f ruta``` se le dice donde quedará la nueva key generada por lo que se omite la pregunta de ubicación ej: ```ssh-keygen -t rsa -f /home/username/.ssh/new-key```

## Create alias for ssh command

Standard ssh client reads the configuration files before any connection:

```bash
/etc/ssh/ssh_config   # system-wide
~/.ssh/config         # per user
```

with the latter with more priority.

Ideally you want to configure the systemwide config with parameters that please all users and the config in your homedir with options specific to you.
Here's an expample of ```~/.ssh/config```

```bash
Host server1
  HostName 10.12.152.200
  User jonh

Host server2
  HostName 25.74.84.203
  User root
  Port 2022
```

Then to connect to server1:

```bash
ssh server1
```

**Note**: ```~/.ssh/config``` must have read/write permissions for the user, and not be accessible by others (```chmod 600 ~/.ssh/config```)

All possible options are documented in ssh_config manual pages: [man ssh_config](https://man.openbsd.org/ssh_config)

## Use multiple SSH private keys on one client

Suppose you have 2 keys

```bash
/home/username/.ssh/id_rsa #default key
/home/username/some_directory/other-ssh-key
```

One way is using the -i param followed by the route to the key, example ```ssh -i /home/username/some_directory/other-ssh-key hostuser@servername```

Other way is configure the alias on the ssh config file (default location) ```/home/username/.ssh/config``` the param ```IdentityFile```, example:

```bash
Host serverAlias
    HostName 12.25.36.175
    IdentityFile ~/some_directory/other-ssh-key  # different private key to the default
    User remoteusername

Host serverAlias2
    HostName server.organism.org
    IdentityFile ~/.ssh/id_rsa  # default private key 
    User username
```
