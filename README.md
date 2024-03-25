
# Introducción a los contenedores

En este repositorio se muestran escenarios de despliegue de servicios típicos que pueden simplificarse enormemente usando contenedores Docker. Todos estos despliegues pueden hacerse de forma tradicional pero con la complejidad que implica instalar, arrancar y configurar cada servicio y dependencia por separado. Los despliegues mostrados son:

* Servicio web **Nginx**
* Gestor de contenidos **Wordpress**
* Gestor de proyectos y código **Gitea**

## ¿Como instalar Docker Engine?

Si estas sobre una máquina virtual de Debian recién instalada y no tienes el Docker Engine instalado puedes abrir sesión como `root`, entrar a este mismo directorio y ejecutar el script `instalar-docker`. Una vez tengas el Docker Engine instalado en tu Debian puede interesarte tener un gestor gráfico para manejar contenedores, imágenes, volúmenes, etc. Uno muy fácil de instalar y usar es [Portainer](https://docs.portainer.io/user/home) que se instala como un contenedor más simplemente ejecutando sobre la máquina anfitriona:

```
docker volume create portainer_data
docker run -d -p 8000:8000 -p 9443:9443 --name portainer --restart=always -v /var/run/docker.sock:/var/run/docker.sock -v portainer_data:/data portainer/portainer-ce:latest
```

Tras esto entramos en https://192.168.1.10:9443 (cambiando la IP por la de la máquina Debian que está funcionando como anfitrión y donde tenemos funcionando el Docker Engine). Asignamos una primera credencial a la contraseña de Administrador y empezamos a usar Portainer. El primer paso es crear un nuevo entorno que nos permita administrar la máquina Debian. Para añadir un entorno nuevo simplemente hemos de ejecutar el comando del "agente" que nos indica en la máquina donde estén los contenedores que queramos instalar. Normalmente la misma máquina. En la instalación sobre linux ya nos crea un primer entorno llamado "local" al que podemos o debemos conectarnos para iniciar la administración via Portainer web.

    

## Referencias:

- Instalación de Docker: https://docs.docker.com/engine/install/debian/
- Instalación de Gitea: https://docs.gitea.com/category/installation
- Instalación de Portainer: https://docs.portainer.io/start/install/server/docker/linux
