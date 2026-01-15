
# Repositorio Debian sobre un contenedor

Lo primero que necesitamos es construir una imagen de nuestro contenedor pues esta no la vamos a encontrar en dockerhub. Para construir esta imagen usaremos el comando:

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
