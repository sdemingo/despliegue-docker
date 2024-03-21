# Instalación de Wordpress

Para instalar e iniciar un blog con Wordpress usando docker debemos de iniciar el servicio en sí, que funciona sobre un servidor web Apache con su propio intérprete de PHP y una base de datos MySQL. Para poner a funcionar todo esto hemos de iniciar un contenedor con el servicio web y la aplicación corriendo en su módulo de PHP y otro contenedor con la base de datos. Para el almacenamiento persistente de ambos contenedores usaremos sendos volúmenes que están guardados bajo `/var/lib/docker/volumes` o bien listándolos con el comando `docker volume ls`.


Para eliminar toda la instalación bastará con parar los contenedores y eliminar los volúmenes creados.
