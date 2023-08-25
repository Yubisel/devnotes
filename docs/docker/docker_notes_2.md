
## Notas

### Espacio usado por docker

`docker system df`

para liberar espacio usar `docker system prune --all` borra los siguientes datos

  - all stopped containers
  - all networks not used by at least one container
  - all images without at least one container associated to them
  - all build cache


### Conocer sobre los recursos utilizados por los contenedores

`docker stats`