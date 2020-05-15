La instalación de Portainer es muy sencilla, ya que se instala como contenedor de Docker.

Antes de ejecutar el contenedor, debemos crear un 'volume'. Éstos son la forma que tiene Docker de crear unidades de almacenamiento persistentes. Es decir, una forma de mantener información aunque el contenedor se pare. También permiten compartir información entre distintos contenedores.

Haz click en el siguiente comando para crear una unidad de almacenamiento persistente llamada 'portainer_data':

`docker volume create portainer_data`{{execute}}

Para iniciar una instancia de Portainer, solo nos queda ejecutar el siguiente comando:

`docker run -d -p 8000:8000 -p 9000:9000 --name=portainer --restart=always -v /var/run/docker.sock:/var/run/docker.sock -v portainer_data:/data portainer/portainer`{{execute}}

Una vez completada la ejecución del comando, la aplicación web de Portainer estará accesible en el puerto 9000 de nuestro servidor. Para acceder a la que hemos creado aquí, puedes darle a la pestaña 'Portainer' a la derecha de 'Terminal' o acceder en una nueva ventana a la dirección: https://[[HOST_SUBDOMAIN]]-9000-[[KATACODA_HOST]].environments.katacoda.com/

Cuando entres por primera vez, pedirá que introduzcas un usuario y contraseña de administración:

![Usuario y contraseña de Portainer](https://raw.githubusercontent.com/DavidLMS/katacoda-scenarios/master/portainer/assets/user-pass-portainer.png)

Una vez los elijas, se mostrarán las opciones de conexión:

![Conexión de Portainer](https://raw.githubusercontent.com/DavidLMS/katacoda-scenarios/master/portainer/assets/conexion-portainer.png)

- Local. Esta opción te permite gestionar los contenedores del servidor en el que has instalado Portainer. Es la opción que vamos a elegir para continuar este curso.
- Remote. Te permite gestionar los contenedores de un servidor remoto.
- Agent. Puedes conectarte a un agente de Portainer dentro de un clúster Swarm usando esta opción.
- Azure. A través de esta opción puedes gestionar contenedores de Azure.

Elige Local y pulsa en 'Connect'.

Por último, aparecerá la ventana principal de Portainer:

![Página principal de Portainer](https://raw.githubusercontent.com/DavidLMS/katacoda-scenarios/master/portainer/assets/principal-portainer.png)

Haz click en 'Local', el único Endpoint disponible, para acceder a la gestión del mismo.