---
tags:
  - CCNA
---
**First Hop Redundancy Protocols (FHRP)** es una categoria de protocolos que permiten que multiples routers puedan proporcionar una [[Default gateway]] redundante para los hosts de una [[LAN]], minimizando el downtime ante una falla de hardware. 

**Tabla de diferencias entre protocolos FHRP**
![[Pasted image 20241205235521.png]]


Para lo cual se usa una _Virtual IP (VIP) address_ compartida entre los routers (estos responden la solicitud [[ARP]] dirigida hacia el VIP), la cual se configura en los end hosts para usar como default gateway. 

![[Pasted image 20241204094229.png]]

Un conjunto de routers configurados para usar un FHRP es llamado un _grupo_. Un único router puede ser parte de multiples grupos FHRP, lo cual es util cuando tenemos multiples subnets en una [[LAN]] porque permite que cada subnet pueda tener un grupo FHRP. 

### FHRP neighbor relationships 
Los routers se coordinan entre sí enviando mensajes _multicast hello_ en intervalos regulares, estos mensajes _hello_ permite establecer FHRP neighbor relationships y luego mantenerlos. 
- Si un router deja de recibir mensajes _hello_, este asume que el vecino esta DOWN y actuara en consecuencia (i.e toma el rol de default gateway)

> Una ventaja de los mensajes _multicast_ frente al _broadcast_ es que cuando se envia un multicast, todos los dispositivos los cuales no esperan un mensaje de este tipo lo descartan automaticamente. En cambio al hacer flooding de un mensaje broadcast, todos los dispositivos que reciben el mensaje realizan el proceso de desencapsulamiento para ver si el paquete resulta ser de su interes y luego descartarlo, lo que implica más consumo de recursos. 

![[Pasted image 20241205015659.png]]

A través de los mensajes hello, los routers eligen el router _active_ / _standby_
- El router _active_ sirve como [[Default gateway]] 
- El router _standby_ esta preparada en caso de que por algún motivo el active router caiga 

Además de compartir una _Virtual IP_, los routers tambien comparten un _Virtual MAC Address_, que no es una MAC de sus interfaces.
- El host que recibe el [[ARP]] reply crea una entrada en su tabla asociando la VIP a la Virtual MAC address. Cuando se envien paquetes hacia destinos externos, el _frame_ tiene como destino la Virtual MAC address (ver: [[How end hosts send packets]])
- Los switches aprenden la Virtual MAC address asociando al puerto del _active_ router. 

### Failover
El proceso de intercambio active / standby por falla es llamada _failover_. Como los routers FHRP comparten la misma IP / MAC address no es necesario realizar cambios en los hosts finales para que el trafico siga su flujo. 

El cambio que si se realiza en este proceso es en la [[MAC address table]] de los switches, ya que necesitan saber en que puerto se encuentra el active router nuevo para realizar el forward de los frames. 
- El nuevo _active_ router envia  mensajes _gratuitous [[ARP]] (GARP)_, estos son [[ARP]] replies que no fueron solicitados por un ARP request. Estos se envian como un mensaje broadcast que inundan toda la red (a diferencia de un ARP request normal, que se envia como unicast)
	- Los source MAC address de estos mensajes GARP son las Virtual MAC address, esto hace que los swiches actualicen su [[MAC address table]] siempre vinculando la Virtual MAC address al puerto correspondiente 

![[Pasted image 20241205235037.png]]
En caso de que el router que era _active_ vuelva a estar operativo:
- Puede reclamar su rol como active router, esto es llamado _preemption_
	- Este término tambien se aplica en [[OSPF]] en la elección de DR / BDR routers 
- Se convierte en el nuevo standby router

