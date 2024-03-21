# Instalación de Nginx

Este contenedor es muy simple. Simplemente levanta un servicio web de tipo Nginx que funciona sobre un Linux de tipo Alpine, en el puerto 8080 de la máquina anfitriona. Además vincula el directorio interno del contenedor donde residen los ficheros HTML que queremos exportar con el directorio `web` que se encuentra en este mismo directorio. Actualizando el contenido de este directorio actualizaremos la web exportada con el servicio del contenedor.
