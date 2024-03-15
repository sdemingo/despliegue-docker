# Instrucciones para el despliegue de Redmine

Este directorio contiene un simple archivo de docker-compose para el despliegue de un Redmine con un MySQL 8.0. Para desplegar la infraestructura simplemente ejecuta el comando `docker compose up` sobre este mismo directorio. Una vez desplegado hemos de entrar en http://localhost:8080 si hemos desplegado los contenedores en nuestra propia máquina o bien cambiar `localhost` por la IP de la máquina donde hayamos desplegado la infraestructura. Para poder entrar con un primer usuario como rol de administrador y comenzar a personalizar la aplicación usaremos el usuario `admin` y la contraseña `admin`.

## ¿Como instalar Docker Engine?

Si estas sobre una máquina virtual de Debian recién instalada y no tienes el
Docker Engine instalado puedes abrir sesión como `root`, entrar a este mismo
directorio y ejecutar el script `install-docker`.

Referencias:

- https://docs.docker.com/engine/install/debian/
- https://hub.docker.com/_/redmine/
