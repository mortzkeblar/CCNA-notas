## Network address 
La dirección de red es la primera address de cualquier red y se utiliza para identificar la red, esta no puede ser asignada a un host. 

![[Pasted image 20241026022934.png|500]]

## Broadcast address 
La dirección broadcast es la ultima dirección de cualquier red, esta no puede ser asignada a un host. Se utiliza para enviar mensajes a todos los hosts que se encuentren un su [[LAN]]. 

![[Pasted image 20241026023130.png|500]]

> Una dirección broadcast de un red especifica puede ser usada para enviar mensajes desde hosts en otra red. 

## Maximum number of hosts 

Este concepto trata sobre la cantidad de direcciones disponibles podemos asignar a los hosts conectados a la red. 

Esta se calcula como $2^{x}-2$ siendo $x$ los bits de de la porción de hosts. Se resta 2 direcciones ya que estas corresponde a la dirección de red y broadcast.  