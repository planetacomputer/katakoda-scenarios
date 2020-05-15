**Instalar** el servicio SSH en un sistema basado en Ubuntu es tan sencillo como ejecutar el siguiente comando:

`sudo apt-get install ssh`{{execute HOST2}}

Para **comprobar** que el servicio se está ejecutando correctamente, utilizaremos:

`sudo service ssh status`{{execute HOST2}}

Si aparece **active (running)** en verde, es que el servicio está en ejecución, por lo que podríamos conectarnos desde cualquier cliente para controlar ese host remotamente.