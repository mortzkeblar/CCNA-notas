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


## Neighbor requirements 
Aunque los [[OSPF network types and DR - DBR election]] permite a los routers descubrir dinamicamente vecinos [[OSPF]]. No garantiza que se puedan formar _neighbor relationships_. Se deben cumplir una serie de requisitos entre las cuales se encuetran.
- Número de [[OSPF area]] coincidente 
- Subnet (network address, netmask), debe coincidir 
- El proceso OSPF no debe ser shutdown
- Los [[OSPF#Router ID]] deben ser  únicos
- Los [[OSPF timers]] _Hello_ y _Dead_ deben ser coincidentes 
- Configuración de autenticación debe coincidir 
	- Comandos `ip ospf authentication` en la interface y `ip ospf authentication-key <password>` para setear una contraseña que permite que solo los routers que hagan match con esa contraseña puedan ser vecinos 
- Configuración IP MTU debe coincidir 
	- El valor MTU de una interface se puede modificar con `ip mtu <bytes>`. El valor por defecto es de 1500 bytes en una interface ethernet.
	- Cuando hay un mismatch error de este tipo, el [[OSPF neighbor states]] no pasara de los estados _ExStart/Exchange_.
- [[OSPF network types and DR - DBR election]] debe coincidir 
	- Aunque surja este problema, los routers pueden formar una _full adjacencies_ pero sus [[OSPF#Link-State Database]] no estaran sincronizado y generara inconsistencias al revisar los estado del vecinos con `show ip ospf neighbor`, por lo cual es necesario revisar el enlace de ambos lados así como la tabla de enrutamiento para verificar si existe algún problema. 

