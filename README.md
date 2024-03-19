# Instrucciones para el despliegue de Gitea

Este directorio contiene un simple archivo de docker-compose para el despliegue de Gitea usando una simple base de datos SQLite. En caso de querer usar MySQL o Postgres se necesita iniciar antes su contenedor. Para desplegar la infraestructura simplemente ejecuta el comando `docker compose up` sobre este mismo directorio. Una vez desplegado hemos de entrar en http://localhost:300 si hemos desplegado el contenedor en nuestra propia máquina o bien cambiar `localhost` por la IP de la máquina donde hayamos desplegado el servicio. Una vez en esa página finalizamos la instalación y asignamos una primera cuenta de administrador.

## ¿Como instalar Docker Engine?

Si estas sobre una máquina virtual de Debian recién instalada y no tienes el Docker Engine instalado puedes abrir sesión como `root`, entrar a este mismo directorio y ejecutar el script `instalar-docker`.


## ¿Y si no quiero usar Docker Engine?

En este mismo directorio encontrarás un [tutorial](./instalacion-sin-docker.md) que te describe los pasos necesarios para instalar Gitea de forma directa en tu sistema Debian.
    
    
## Referencias:

- Instalación de Docker: https://docs.docker.com/engine/install/debian/
- Instalación de Gitea: https://docs.gitea.com/category/installation
