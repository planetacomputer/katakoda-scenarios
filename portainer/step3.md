En nuestra instalación de Docker podemos descargar tantas imágenes como espacio de almacenamiento tengamos. Para que nos entendamos, una imagen de Docker es como tener una receta y acceso ilimitado a los ingredientes que queramos. Cuando arrancamos un contenedor, lo que hacemos en poner en práctica esa receta y sacar un plato.

Podemos crear nuestras propias imágenes o descargar imágenes preparadas de algún registro como [Docker Hub](https://hub.docker.com). Ahí encontraremos imágenes preparadas de los servicios y aplicaciones más populares.

En nuestra instancia de Portainer, accede al apartado [images](https://[[HOST_SUBDOMAIN]]-9000-[[KATACODA_HOST]].environments.katacoda.com/#/images), para ver un listado de las imágenes descargadas en nuestra máquina.

![Listado de imágenes](https://raw.githubusercontent.com/DavidLMS/katacoda-scenarios/master/portainer/assets/images-portainer.png)

Puedes comprobar cómo aparece la imagen de Portainer y otras que vienen por defecto con la etiqueta 'Unused'. Eso quiere decir que no están siendo utilizadas por ningún contenedor actualmente. Podríamos, si quisiéramos, eliminarlas para ahorrar espacio en disco. También podemos ver su etiqueta, tamaño y fecha en la que se creó.

Vamos a descargar en la Terminal una nueva imagen de, por ejemplo, el servidor web nginx, para comprobar que aparece en el listado. Para ello ejecutamos el siguiente comando:

`docker pull nginx`{{execute}}

Veremos cómo aparece en el [listado](https://[[HOST_SUBDOMAIN]]-9000-[[KATACODA_HOST]].environments.katacoda.com/#/images) de Portainer si lo actualizamos (el botón para actualizarlo está al lado del nombre en la pestaña).

![Listado de imágenes actualizado](https://raw.githubusercontent.com/DavidLMS/katacoda-scenarios/master/portainer/assets/listado-actualizado-portainer.png)

Vamos a probar a descargar una imagen directamente desde la interfaz de Portainer, en lugar de utilizar el comando. Para ello, vamos a poner el nombre de la imagen en el apartado 'image' del bloque 'Pull image'. Descargaremos por ejemplo una imagen de la aplicación Wordpress.

![Descarga de la imagen de Wordpress](https://raw.githubusercontent.com/DavidLMS/katacoda-scenarios/master/portainer/assets/wordpress-portainer.png)

Pulsamos en 'Pull the image' y esperamos a que se descargue. Podemos comprobarlo en el [listado](https://[[HOST_SUBDOMAIN]]-9000-[[KATACODA_HOST]].environments.katacoda.com/#/images) actualizado.

![Listado de imágenes actualizado con Wordpress](https://raw.githubusercontent.com/DavidLMS/katacoda-scenarios/master/portainer/assets/listado-actualizado2-portainer.png)

También podemos ver el listado de imágenes en la Terminal ejecutando el comando:

`docker image ls`{{execute}}