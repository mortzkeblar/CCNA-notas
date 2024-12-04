---
tags:
  - routing
  - dynamic
  - OSPF
  - CCNA
date created: Saturday, October 19th 2024, 11:08:14 pm
date modified: Wednesday, November 20th 2024, 11:33:50 pm
---

En [OSPF](OSPF.md), adyancencia se refiere a la relación establecida entre dos routers vecinos conectados directamente. Esta relación permite el intercambio de enrutamiento como updates de LSAs para calcular las mejores rutas en la red. 

La formula para calcular las adyancencias en una red es $n(n-1)/2$ (siendo $n$ la cantidad de routers en la red). 

**OSPF message types**

![[Pasted image 20241201231128.png]]

## Neighbor states 
En [[OSPF]] para intercambiar información, los dispositivos pasan por una serie de pasos llamados _neighbor states_ en el que se verifican varios estados de configuración. Todos estos detalles se explican en [[OSPF neighbor states]]. 

![[Pasted image 20241201231450.png]]
Cuando OSPF se activa en una interface, esta comienza a enviar mensajes _OSPF hello_, el cual se usa para  descubrir vecinos OSPF de forma dinamica  y tambien mantener los _neighbor relationships_ una vez establecidos. 
- Los mensajes _OSPF hello_ se envian a la IP `224.0.0.5`
	- La IP 224.0.0.5 es una multicast [[IP address]], que tiene una relacion _one-to-multiple_ (no necesariamente ALL).
	- Una vez se envia este paquete, al pasar por un switch este hace flooding del mismo. Estas son recibidas solo por los routers que tienen la interfaces OSPF activa. 