>Situación de ejemplo:
> Si tenemos varios hosts conectados a un switch, ese switch a su vez esta conectado a un router. Entendemos que el router actua de servidor DHCP. 
> Ya sabemos que si un host quiere conectarse a la red, envia un mensaje broadcast a toda la red. En este caso el router deberia poder responder correctamente.
> En una situación comun de _vida real_, un ISP suele tener muchas subredes pero solo un servidor DHCP en la red (local o remota). Para poder controlar todas las direcciones de manera centralizada. 
> -  Asuminos el el DHCP server tiene la IP `200.1.1.1`
> - Una de las LAN tiene un red `10.0.0.0/24`, el router conectada a esa LAN tiene `10.0.0.1`. 
> 	- Si un host no configurado se conecta a LAN anteriormente descrita y envia un _broadcast_. Ese mensaje solo va a llegar hasta el router que esta en `.1` (así como cualquier mensaje dentro de la LAN, no puede pasar del router). 
> 		- El problema es esta situación es que el servidor DHCP esta en una ubicación remota fuera de su red LAN. Entonces el mensaje en principio no puede llegar hasta el DHCP server.
>


La solución que plantea DHCP relay es:
 - La interfaz en la que el router recibe el mensaje _broadcast_ del host solicitante (la interfaz conectada a la LAN). El router recibe el mensaje y lo convierte en _unicast_, la cual se envía hasta llegar al servidor DHCP.
 - El servidor DHCP responde el mensaje con otro mensaje _unicast_ al router, el cual se lo reenvia al host por el mismo metodo _unicast_, así hasta que concluye el proceso con el host configurado (o rechazado por el servidor).
 
 ![](../_anexos_/Screenshot%20from%202024-01-01%2021-30-07.png)

**Códigos DHCP**
 > Considerar que DHCP se rige netamente por el uso de codigos.


https://www.iana.org/assignments/bootp-dhcp-parameters/bootp-dhcp-parameters.txt

