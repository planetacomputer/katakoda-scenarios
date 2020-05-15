Para un administrador de sistemas, el **acceso remoto** es fundamental, ya que permite:

* Administrar y resolver problemas en un servidor o equipo sin necesidad de estar físicamente presente.
* Asesorar y ofrecer apoyo técnico, así como soluciones de problemas informáticos a usuarios, sin realizar ningún desplazamiento.

**SSH** (Secure Shell) es un **protocolo** de acceso remoto por **línea de comandos** seguro, en el que la comunicación se encuentra **cifrada**. Por defecto, la conexión se realiza a través del puerto **22 TCP**.

El esquema de funcionamiento de este protocolo es muy sencillo:

<center>![Esquema de acceso remoto](https://raw.githubusercontent.com/DavidLMS/katacoda-scenarios/master/ssh/assets/esquema-acceso-remoto.png)</center>

En el servidor instalaremos **el servicio SSH** que permita el control remoto. En el cliente, que servirá para controlar el servidor, instalaremos el **software cliente** que permitirá acceder al servidor.

En esta práctica, **el servidor será el Host 2** (Terminal inferior) y **el cliente el Host 1** (Terminal superior). Accederemos, por tanto, al Host 2 desde el Host 1.

Primero debemos tener claro las **direcciones IP** de ambos hosts. Para ello, ejecutamos el comando **ip add**.

Haz click en el siguiente comando para ejecutarlo en el Host 1 (cliente):

`ip add`{{execute HOST1}}

Haz click en el siguiente comando para ejecutarlo en el Host 2 (servidor):

`ip add`{{execute HOST2}}

La IP es la que aparece a continuación de **inet** en la tarjeta de red **ens3** de cada uno de los hosts.

También debemos comprobar que un host **tiene conectividad** con el otro (porque sino, no podremos hacer una conexión SSH).

Prueba a hacer **ping** desde el cliente (Host 1) al servidor (Host 2) usando el siguiente comando:

`ping -c 4 [[HOST2_IP]]`{{execute HOST1}}

Aunque no es necesario, también podrías hacer **ping** desde el servidor (Host 2) al cliente (Host 1) usando el siguiente comando:

`ping -c 4 [[HOST_IP]]`{{execute HOST2}}

Ya lo tenemos todo listo para comprender y realizar esta práctica. Continúa para preparar el servidor.

APUNTES
apt update

apt install vsftpd -y
useradd -g ftp -s /bin/bash usuarioftp
passwd usuarioftp
mkdir /home/usuarioftp
chown -R usuarioftp:ftp /home/usuarioftp

apt install nginx -y
apt install php -y
cd /var/www/html/
wget https://github.com/DavidLMS/katacoda-scenarios/raw/master/ftp/assets/monsta_ftp_2.10.1_install.zip
unzip monsta_ftp_2.10.1_install.zip
cp -R mftp/* ./
rm -R mftp
rm monsta_ftp_2.10.1_install.zip

nano /etc/nginx/sites-enabled/default -> Añadir index.php a la lista y descomentar lo de PHP (segunda opción)
service nginx restart