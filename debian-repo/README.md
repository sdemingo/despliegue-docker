
# Repositorio Debian sobre un contenedor

Vamos a construir un repositorio que funcione sobre un contenedor Docker. Este repositorio lo vamos a construir sobre Debian y va tener, además de su estructura de ficheros base los siguientes directorios:

```
/
 │
 ├─ repo/
 │    ├─ dists/
 │    ├─ pool/
 │    └─ conf/
 │        └─ distributions
 │
 ├─ etc/
 │    └─ nginx/
 │         └─ nginx.conf
 │
 └─ build/
      └─ paqprueba.deb
```

El directorio `repo` y todo su contenido es básicamente la estructura fundamental del repositorio y su configuración. Será creada por el scritp `reprepro` que viene instalado en nuestra Debian. Además tendremos la configuración del servidor web `nginx` que la tomamos del fichero `nginx.conf`de este mismo directorio y que instalaremos dentro del directorio `/etc/nginx/nginx.conf` de nuestro contenedor. Para terminar, todos los paquetes que queramos servir de forma adicional a la caché ofrecida por el servidor (como es el caso del paquete `paqprueba` irán contenidos en el directorio `/build`. 

Como la imagen tal y como la necesitamos no se encuentra en Dockerhub la tenemos que crear a partir del fichero `Dockerfile` ubicado en este mismo directorio. Para construir esta imagen usaremos el comando:

```
docker build --no-cache -t debian-repo .
```

Después debemos lanzar un contenedor que use esta imagen. Este contenedor no lleva un volumen asociado. La imagen está construida de tal forma que en su interior incorpora un paquete llamado `paqprueba` que simula ser nuestra aplicación privada que queremos distribuir a través de `apt`.

Para ahora lanzar este contenedor con esta imagen usamos:

```
docker run -d --name debian-repo -p 8080:80 debian-repo
```

## Configurar tu repositorio en el cliente

Usaremos nuestra propia máquina host como cliente de este repositorio. Para ello, solo necesitamos agregar el repositorio o bien editando al final del fichero `/etc/apt/sources.list` o bien añadiendo la configuración propia de este repositorio en un fichero separado en `/etc/apt/sources.list.d`. Por limpieza, haremos esto segundo:
```
# echo "deb [trusted=yes] http://localhost:8080 stable main" | tee /etc/apt/sources.list.d/mirepo.list
```

Con esto solo necesitamos recargar los catálogos, incluyendo el del repositorio que acabamos de configurar e instalar el paquete de prueba que tenemos esperando en el repositorio:

```
# apt update
# apt install paqprueba
```

Ejecutamos el nuevo comando instalado y vemos si funciona:

```
$ paqprueba
Hola desde paqprueba. Si lees esto es que todo ha funcionado :)
```
