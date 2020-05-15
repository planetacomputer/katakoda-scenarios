Si queremos **pasar un archivo del cliente al servidor o viceversa**, podemos utilizar **SCP**, que es un programa que **usa SSH** para la **transferencia de ficheros**.

Podemos **descargar un archivo al cliente desde el servidor** utilizando el siguiente comando:

<pre>scp USUARIO_SERVIDOR@IP_SERVIDOR:RUTA_ARCHIVO/NOMBRE_ARCHIVO_EN_SERVIDOR NOMBRE_ARCHIVO_EN_CLIENTE</pre>

Donde:
* USUARIO_SERVIDOR es el nombre de usuario del servidor
* IP_SERVIDOR es la dirección IP del servidor
* RUTA_ARCHIVO es la ruta (localización) del archivo en el servidor
* NOMBRE_ARCHIVO_EN_SERVIDOR es el nombre del archivo en el servidor
* NOMBRE_ARCHIVO_EN_CLIENTE es el nombre que queremos que tenga el archivo en el cliente (se descargará en la carpeta donde estemos ejecutando SCP)

Por ejemplo, podemos probar a traernos el archivo **prueba** que creamos en el paso anterior al cliente, llamándolo **recibido** (teniendo en cuenta que ambos hosts se encuentran en la carpeta /root. Puedes asegurarte con el comando **pwd**):

`scp root@[[HOST2_IP]]:/root/prueba recibido`{{execute HOST1}}

Comprobamos que **se ha descargado** y lo tenemos en la carpeta **actual**:

`ls`{{execute HOST1}}

Para **enviar un archivo desde un cliente al servidor**, usamos el comando:

<pre>scp NOMBRE_ARCHIVO_EN_CLIENTE USUARIO_SERVIDOR@IP_SERVIDOR:RUTA_ARCHIVO/NOMBRE_ARCHIVO_EN_SERVIDOR</pre>

Donde:
* USUARIO_SERVIDOR es el nombre de usuario del servidor
* IP_SERVIDOR es la dirección IP del servidor
* RUTA_ARCHIVO es la ruta (localización) del servidor en la que queremos poner el archivo
* NOMBRE_ARCHIVO_EN_SERVIDOR es el nombre que queremos que tenga el archivo en el servidor
* NOMBRE_ARCHIVO_EN_CLIENTE es el nombre del archivo en el cliente. Si no ponemos la ruta, deberá estar en la carpeta en la que estamos ejecutando SCP.

Vamos a enviar al servidor el archivo **recibido**, manteniendo esta vez el nombre:

`scp recibido root@[[HOST2_IP]]:/root/recibido`{{execute HOST1}}

Vamos a **comprobar** que se ha enviado correctamente:

`ls`{{execute HOST2}}

Esta es una forma **muy rápida y simple** de intercambiar archivos entre un cliente y un servidor.
