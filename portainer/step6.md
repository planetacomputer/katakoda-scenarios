Los contenedores de Docker son volátiles por definición. Esto quiere decir que si en algún momento un contenedor se elimina y vuelve a arrancarse una instancia de la misma imagen, todos los datos que hayamos registrado en ese contenedor mientras ha estado ejecutándose desaparezcan.

Por ese motivo, Docker ofrece varias opciones de persistencia (esto es, almacenamiento que no se elimine si desaparece el contenedor). Una de ellas, son los 'volumes', en los que nos vamos a centrar en este apartado.

Al igual que se puede mapear un puerto del anfitrión con un puerto del contenedor, también podemos mapear una carpeta del anfitrión con una carpeta del contenedor, de forma que cuando el contenedor desaparezca la carpeta siga ahí por si volvemos a crear una instancia de la misma imagen. Si queremos guardar una copia de seguridad, solo tendremos que hacer una copia de esa carpeta.

Cuando tenemos un contenedor que almacena algún tipo de dato, no nos va a interesar que permanezcan todas las carpetas del mismo, sino solamente las que vamos a utilizar para almacenar nueva información. Por ejemplo, si tenemos un contenedor con un servicio web como nginx, lo normal es que no nos interesen todas las carpetas comunes del sistema, sino aquellas que guardan la configuración de nginx y las que contienen los archivos de nuestras páginas web.

Podemos ver los volumes creados en la Terminal ejecutando el siguiente comando:

`docker volume ls`{{execute}}

Dentro de Portainer, los volumes creados los encontramos en el apartado [volumes](https://[[HOST_SUBDOMAIN]]-9000-[[KATACODA_HOST]].environments.katacoda.com/#/volumes).

![Lista de volumes](https://raw.githubusercontent.com/DavidLMS/katacoda-scenarios/master/portainer/assets/volumes-portainer.png)

Podemos comprobar que solamente tenemos el volume que contiene los datos persistentes de la aplicación Portainer.

Vamos a hacer una comprobación de la persistencia de los datos en nuestro contenedor servidorweb. Para ello, vamos a agregar una página web al servidor nginx y comprobaremos si se mantiene al eliminar y volver a ejecutar un contenedor de la misma imagen.

Primero, vamos a ejecutar lo siguiente que nos va a permitir acceder al interior del contenedor servidorweb y ejecutar comandos como si accediéramos por SSH al mismo:

`docker exec -it servidorweb bash`{{execute}}

A partir de ahora, los comandos que ejecutemos se realizarán dentro del contenedor servidorweb. 

Prueba a acceder al puerto 80 del anfitrión, que está mapeado al puerto 80 del contenedor, para ver la página web por defecto de nginx: https://[[HOST_SUBDOMAIN]]-80-[[KATACODA_HOST]].environments.katacoda.com

A continuación, vamos a acceder a la carpeta donde se almacena la página web por defecto:

`cd /usr/share/nginx/html`{{execute}}

Eliminamos los archivos que contiene:

`rm *`{{execute}}

Y tendremos que poner ahí dentro una nueva página. Vamos a decargar una con wget, pero primero necesitamos instalarlo:

`apt update`{{execute}}

`apt install wget -y`{{execute}}

Una vez instalado, podemos utilizarlo para descargar el HTML de la página https://davidlms.github.io/:

`wget https://davidlms.github.io/`{{execute}}

Ya hemos terminado de ejecutar comandos en el contenedor, así que salimos del mismo:

`exit`{{execute}}

Puedes comprobar que la nueva web está activa accediendo de nuevo al enlace (puede ser que el caché de tu navegador provoque que se cargue la web antigua, prueba a forzar una recarga pulsando Ctrl y el botón de recargar en Windows o Command y el botón de recargar en macos o abre una nueva ventana privada o prueba en un navegador diferente) https://[[HOST_SUBDOMAIN]]-80-[[KATACODA_HOST]].environments.katacoda.com.

Ahora vamos a eliminar el contenedor servidorweb y volver a crearlo. Para eso, primero accedemos al apartado [containers](https://[[HOST_SUBDOMAIN]]-9000-[[KATACODA_HOST]].environments.katacoda.com/#/containers) y pulsamos en el contenedor servidorweb.

Tenemos dos opciones, o bien eliminamos el contenedor con 'Remove' y creamos uno nuevo como hemos visto anteriormente, o pulsamos el botón 'Recreate' que realiza eso mismo en un solo paso.

Una vez recreado el contenedor, accedemos al enlace https://[[HOST_SUBDOMAIN]]-80-[[KATACODA_HOST]].environments.katacoda.com/ para comprobar que la web que pusimos se ha eliminado y vuelve a aparecer la página por defecto (recuerda recargar el navegador correctamente).

Ya ha terminado nuestra comprobación, así que ahora lo haremos con persistencia.

Para ello, crearemos un volume en el apartado [volumes](https://[[HOST_SUBDOMAIN]]-9000-[[KATACODA_HOST]].environments.katacoda.com/#/volumes).

Pulsamos en 'Add volume'.

![Creando un volume](https://raw.githubusercontent.com/DavidLMS/katacoda-scenarios/master/portainer/assets/volume-creation-portainer.png)

Le ponemos de nombre webNginx y seleccionamos el Driver local. Pulsamos en 'Create the volume' y veremos que aparece listado.

![Volume creado](https://raw.githubusercontent.com/DavidLMS/katacoda-scenarios/master/portainer/assets/volume-created-portainer.png)

Ahora volvemos al apartado [containers](https://[[HOST_SUBDOMAIN]]-9000-[[KATACODA_HOST]].environments.katacoda.com/#/containers), entramos en el contenedor servidorweb y pulsamos en 'Stop' para pararlo y 'Remove' para eliminarlo.

A continuación, crearemos uno nuevo a partir de la misma imagen, pero con persistencia. Pulsamos en 'Add container'.

![Creando un contenedor con persistencia](https://raw.githubusercontent.com/DavidLMS/katacoda-scenarios/master/portainer/assets/persistent-container-portainer.png)

Ponemos de nombre 'servidorwebpersistencia', elegimos la imagen 'nginx:latest', mapeamos el puerto 80 del contenedor al puerto 80 del servidor como ya hicimos previamente y en la pestaña 'Volumes' del bloque de opciones avanzadas pulsamos en 'map additional volume' para enlazar un volume persistente al contenedor. En concreto, le indicamos que la carpeta /usr/share/nginx/html del contenedor (que es la que contiene la web por defecto) la almacene en el volume webNginx que hemos creado anteriormente. Por último, pulsamos en 'Deploy the container'.

Solo nos queda poner la página web de nuevo en este contenedor, como ya hicimos antes:

`docker exec -it servidorwebpersistencia bash`{{execute}}

`cd /usr/share/nginx/html`{{execute}}

`rm *`{{execute}}

`apt update`{{execute}}

`apt install wget -y`{{execute}}

`wget https://davidlms.github.io/`{{execute}}

`exit`{{execute}}

Si accedemos a la dirección https://[[HOST_SUBDOMAIN]]-80-[[KATACODA_HOST]].environments.katacoda.com (recargada convenientemente), veremos de nuevo la página web personalizada. Puedes probar a recrear este contenedor y comprobarás como, a diferencia del anterior, permanece la web intacta.

El uso de volumes es muy interesante, porque nos permite hacer copias de seguridad ocupando el espacio mínimo en disco (ya que solo hacemos copia de lo que nos interesa mantener) y facilita la migración de un contenedor o aplicación de un servidor a otro, ya que solamente necesitaríamos trasladar esa carpeta.
