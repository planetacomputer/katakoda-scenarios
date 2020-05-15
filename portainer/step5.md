El sistema de gestión de Docker puede crear una o varias redes independientes. Por lo tanto, dos contenedores en el mismo servidor pueden estar en redes aisladas entre sí o compartir el mismo direccionamiento IP, dependiendo de nuestras necesidades y requisitos de seguridad.

Para ver las redes en Portainer, accedemos al apartado [networks](https://[[HOST_SUBDOMAIN]]-9000-[[KATACODA_HOST]].environments.katacoda.com/#/networks).

![Listado de redes](https://raw.githubusercontent.com/DavidLMS/katacoda-scenarios/master/portainer/assets/networks-portainer.png)

Por defecto, hay tres redes creadas por el sistema, el nombre es indiferente. De momento solamente nos interesa la columna 'Driver'. En Docker, las redes creadas pueden basarse en distintos controladores, los dos que más nos interesan ahora son:

- bridge. Es el controlador por defecto. Si no se indica nada cuando se pone en marcha el contenedor, se le ubica en una red de este controlador. Como puedes ver, se indica una subred en la red de este tipo. Crea una red aislada del anfitrión (donde está instalado Docker), que puede ser compartida por varios contenedores para comunicarse entre ellos.
- host. Elimina el aislamiento entre el contenedor y el anfitrión, usando directamente la misma red que el anfitrión, como si fuera otro equipo que formara parte de la misma.

También puedes ver las redes creadas en la Terminal, usando el comando:

`docker network ls`{{execute}}

Haz click en la red bridged del apartado [networks](https://[[HOST_SUBDOMAIN]]-9000-[[KATACODA_HOST]].environments.katacoda.com/#/networks) en Portainer para ver más detalles.

![Detalles de la red bridged](https://raw.githubusercontent.com/DavidLMS/katacoda-scenarios/master/portainer/assets/bridged-portainer.png)

En el bloque 'Containers in network' puedes ver los contenedores asociados a esta red y su IP. Según se muestra en la imagen anterior, el contenedor servidorweb y portainer están en la misma subred. Vamos a comprobarlo haciendo ping desde servidorweb a portainer.

Para ello, ejecutamos el siguiente comando para acceder a una consola dentro del contenedor servidorweb:

`docker exec -it servidorweb bash`{{execute}}

Todo los comandos que pongamos a partir de ahora hasta poner 'exit' se ejecutarán dentro del contenedor.

Vamos a instalar los paquetes necesarios para poder usar ifconfig y ping:

`apt update`{{execute}}

`apt install net-tools`{{execute}}

`apt install iputils-ping -y`{{execute}}

Comprueba ahora la IP que tiene el contenedor (aparecía dentro de la red bridged en Portainer):

`ifconfig`{{execute}}

Prueba a hacerle ping al contenedor portainer para comprobar que tienen conectividad:

`ping -c 4 172.18.0.2`{{execute}}

Salimos de la terminal del contenedor ejecutando:

`exit`{{execute}}

Ahora vamos a crear una red para aislar los dos contenedores entre sí. Mientras no necesiten tener conexión por algún servicio, es una práctica recomendada de seguridad. Dentro del apartado [networks](https://[[HOST_SUBDOMAIN]]-9000-[[KATACODA_HOST]].environments.katacoda.com/#/networks) en Portainer, pulsamos en 'Add network'.

![Añadir una red](https://raw.githubusercontent.com/DavidLMS/katacoda-scenarios/master/portainer/assets/add-network-portainer.png)

Vamos a completar los siguientes apartados:

- Name: redNginx
- Driver: bridge
- Subnet: 192.168.1.0/24
- IP range: 192.168.1.0/24
- Gateway: 192.168.1.1

Y pulsamos en 'Create the network'.

Accedemos ahora al apartado [containers](https://[[HOST_SUBDOMAIN]]-9000-[[KATACODA_HOST]].environments.katacoda.com/#/containers) en Portainer. Y hacemos click en el contenedor servidorweb. Pulsamos en 'Stop' para pararlo mientras lo cambiamos de red. Buscamos el apartado 'Connected networks'.

![Redes conectadas a servidorweb](https://raw.githubusercontent.com/DavidLMS/katacoda-scenarios/master/portainer/assets/connected-network-portainer.png)

Pulsamos en 'Leave network'. 

Después, en el desplegable que aparece 'Select a network' elegimos la red 'redNginx' creada previamente.

![Redes elegibles para servidorweb](https://raw.githubusercontent.com/DavidLMS/katacoda-scenarios/master/portainer/assets/choose-network-portainer.png)

Pulsamos en 'Join network'.

![servidorweb unido a la red redNginx](https://raw.githubusercontent.com/DavidLMS/katacoda-scenarios/master/portainer/assets/join-network-portainer.png)

Una vez estamos en la nueva red, ya no deberíamos tener conectividad con el contenedor portainer. Vamos a volver a hacerle ping para comprobarlo, pero esta vez accederemos a una consola del contenedor servidorweb por la interfaz gráfica de Portainer.

Para ello, primero pulsamos en 'Start' para arrancar el contenedor y después accedemos al apartado [containers](https://[[HOST_SUBDOMAIN]]-9000-[[KATACODA_HOST]].environments.katacoda.com/#/containers).

![Botón para acceder a la consola de servidorweb](https://raw.githubusercontent.com/DavidLMS/katacoda-scenarios/master/portainer/assets/servidorweb-console-portainer.png)

Pulsamos en el acceso directo a la consola del contenedor (símbolo marcado en la imagen anterior).

![Accediendo a la consola de servidorweb](https://raw.githubusercontent.com/DavidLMS/katacoda-scenarios/master/portainer/assets/servidorweb-console2-portainer.png)

Dejamos las opciones por defecto y pulsamos en 'Connect'.

![Consola de servidorweb](https://raw.githubusercontent.com/DavidLMS/katacoda-scenarios/master/portainer/assets/servidorweb-console3-portainer.png)

Nos aparecerá una consola en la interfaz y podremos intentar hacer ping a portainer escribiendo:

ping -c 2 172.18.0.2

Podrás comprobar que ahora no tiene conectividad, por lo que los contenedores están aislados en redes distintas. También puedes ejecutar el comando ifconfig para conocer la IP actual, que será una del rango que definimos en la red redNginx que creamos previamente.

