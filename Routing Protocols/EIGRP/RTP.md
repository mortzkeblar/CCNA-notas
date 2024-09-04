Reliable Transport Protocol (RTP), es un protocolo usado por [[EIGRP]] para asegurar que los paquetes son enviados, recibidos, ordenados y reconocidos. Esta es la forma en la que EIGRP garantiza el envio y confiabilidad de los paquetes enrutados.  

> RTP no usa TCP o UDP para el proceso 

Cuando un paquete multicast es recibido, este se responde en forma de un mensaje unicast. Además, cualquier actualización debe enviarse con un _sequence number_ para asegurar que se mantiene el orden correcto. 

EIGRP usa cinco tipos de paquetes (via IP protocol 88), cada uno tiene un codigo de operación que es un campo de 4-bit donde se especifica el mensaje EIGRP, estos son:
- Hello (5)
- Ack
- Update (1)
- Query (3)
- Reply (4)
- IPX SA (6)

![[Pasted image 20240904030956.png]]
