# Docker

## Hacer cat a fichero dentro de contenedor

Esto es util para cuando hace falta el contenido de un fichero dentro del container por ejemplo luego de una instalacion de jenkins 

```bash
docker exec jenkins cat /var/jenkins_home/secrets/initialAdminPassword
```
