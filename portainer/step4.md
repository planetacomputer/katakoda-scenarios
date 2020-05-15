Una vez tenemos una imagen descargada, podemos ejecutar tantos contenedores de la misma como queramos. Al igual que si tenemos una receta e ingredientes, podemos hacer tantos platos siguiendo la misma como queramos.

Para entendernos, aunque es una comparación muy inexacta, un contenedor ejecutándose es similar a una pequeña máquina virtual con los recursos justos para ejecutar una aplicación o servicio concretos (o varias aplicaciones o servicios concretos).

En nuestra instancia de Portainer, podemos ver el listado de contenedores en el apartado [containers](https://[[HOST_SUBDOMAIN]]-9000-[[KATACODA_HOST]].environments.katacoda.com/#/containers). De momento, solo está ejecutándose el contenedor de la aplicación Portainer. Desde aquí podemos pararlo, eliminarlo, ver estadísticas, acceder a una terminal del mismo... Además, vemos mucha información como su estado, la IP, el mapeo de puertos o su fecha de creación.

![Listado de contenedores](https://raw.githubusercontent.com/DavidLMS/katacoda-scenarios/master/portainer/assets/containers-portainer.png)

A continuación, vamos a crear un nuevo contenedor de la imagen previamente descargada de nginx en Portainer. Pulsamos en 'Add a container'.

![Creación container nginx](https://raw.githubusercontent.com/DavidLMS/katacoda-scenarios/master/portainer/assets/nginx-container-portainer.png)

Ponemos el nombre que queramos al contenedor, por ejemplo, servidorweb (no puede contener espacios). En 'Image' indicamos el nombre de la imagen, en nuestro caso 'nginx:latest'. Como es un servidor web, para poder acceder al mismo necesitamos exponer el puerto 80, que es donde por defecto estará configurado el servidor http. Para ello pulsamos en 'publish a new network port' y ponemos que el puerto 80 de nuestra máquina redirigirá al puerto 80 del contenedor. Vamos a marcar la opción 'Auto remove' para que el contenedor se elimine automáticamente cuando lo paremos. Por último, pulsamos en 'Deploy the container'.

Vamos a comprobar en la Terminal que está en marcha el contenedor ejecutando el comando:

`docker container ls`{{execute}}

Entra ahora en el apartado [containers](https://[[HOST_SUBDOMAIN]]-9000-[[KATACODA_HOST]].environments.katacoda.com/#/containers) de nuevo en Portainer.

![Listado de contenedores actualizados](https://raw.githubusercontent.com/DavidLMS/katacoda-scenarios/master/portainer/assets/containers-actualizado-portainer.png)

Selecciona el contenedor 'servidorweb' y pulsa en 'Stop' para pararlo. Comprueba en la Terminal que se ha eliminado como indicamos ejecutando el comando de nuevo:

`docker container ls`{{execute}}

Ahora vamos a crear un contenedor de nuevo de la misma imagen, pero a través de comandos, para que compruebes cómo se hace:

`docker run --name servidorweb -d -p 80:80 nginx`{{execute}}

La única diferencia de este contenedor con el que creamos antes, es que no hemos puesto que se elimine al pararlo. Puedes comprobar que el servidor web está funcionando accediendo al puerto 80 en la web https://[[HOST_SUBDOMAIN]]-80-[[KATACODA_HOST]].environments.katacoda.com.