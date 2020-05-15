En la mayoría de sistemas **Linux y macOS**, el software cliente para iniciar una conexión ssh viene **instalado por defecto**. En sistemas Windows es probable que se incluya próximamente, de momento se puede [activar de forma opcional](https://docs.microsoft.com/es-es/windows-server/administration/openssh/openssh_install_firstuse) o utilizar un software externo como [Putty](https://www.putty.org/).

Para **iniciar una conexión** SSH desde un cliente, escribimos el comando

<pre>ssh USUARIO_SERVIDOR@IP_SERVIDOR</pre>

**Sustituyendo** USUARIO_SERVIDOR por el **nombre de usuario** del servidor al que deseamos conectarnos e IP_SERVIDOR por la **dirección IP** del servidor. Cuando intentemos la conexión, nos pedirá la **contraseña** de ese usuario.

En este caso, el nombre de usuario del servidor es **root** y **no tiene contraseña**, por lo que no nos la pedirá.

Vamos a probar:

`ssh root@[[HOST2_IP]]`{{execute HOST1}}

La **primera vez** (y solo la primera) que nos conectemos a un servidor, nos aparecerá un mensaje similar al siguiente:

<pre>The authenticity of host 'IP_SERVIDOR (IP_SERVIDOR)' can't be established.
ECDSA key fingerprint is SHA256:EthPlzQHuXJkqNMN9TBCC2tX87Xjswpmy1Y4dkUY1iE.
Are you sure you want to continue connecting (yes/no)?</pre>

Si escribimos **yes** agregará la clave del servidor a una lista segura. Si en algún momento el host que tiene esa IP cambia, detectará que no es el mismo equipo y aparecerá un mensaje denegándote la conexión por seguridad.

Prueba a escribir yes y se conectará al servidor:

`yes`{{execute HOST1}}

**Aparentemente**, no verás nada distinto, pero **los comandos que ejecutes en el cliente** (Host 1 superior), **será como si los ejecutases en el servidor** (Host 2 inferior). Para comprobarlo, vamos a **crear un archivo de prueba** en el servidor desde el cliente:

`touch prueba`{{execute HOST1}}

Y ahora vamos a **verificar** que se ha creado en el servidor:

`ls`{{execute HOST2}}

Si queremos **cerrar la conexión**, solamente tendremos que ejecutar en el cliente:

`exit`{{execute HOST1}}

Podemos ver que en el cliente no existe el archivo prueba:

`ls`{{execute HOST1}}

¡Ya puedes conectarte a otros ordenadores a través de la red! Usa tu poder con responsabilidad.