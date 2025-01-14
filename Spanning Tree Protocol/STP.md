---
tags:
  - CCNA
  - SpanningTreeProtocol
date created: Sunday, November 10th 2024, 8:03:11 pm
date modified: Wednesday, November 20th 2024, 11:33:50 pm
---
_Spannign Tree Protocol (STP)_, es un protocolo (definido en el standard IEEE 802.1D) que ayuda a resolver problemas de tipo _Layer 2 loops_ dentro de una [[LAN]], estos son frames que estan en loop dentro de la red de forma indefinida. 

> El problema que ataca STP es similar a [[TTL]] en la [[IPv4 Header]]. 

Este problema suele aparecer principalmente cuando se trata de introducir conexiones redundantes en la red. Se puede identificar dos problemas principales: _broadcast storn_ y _MAC address flapping_.


![[Pasted image 20241110201000.png]]

- *Broadcast storn* - se conoce como tormenta broadcast cuando la cantidad de frames que hacen loop en la red. Comienzan a saturar al punto de consumir muchos recursos como CPU o ancho de banda haciendo que la red se inutilizable.
- *MAC address flapping* - este problema ocurre cuando un [[switch]] aprende la misma MAC address desde dos puertos diferentes. 

Spanning Tree Protocol se puede resumir como _la prevención de Layer 2 Loops mediante el bloqueo de la conexiones redundantes  a nivel de  la topologia LÓGICA (no FÍSICA), manteniendo una unica ruta activa entre dos nodos cualquieras en un una [[LAN]]_. 

> Es importante aclarar que los switches Cisco ejecutan una versión propietaria del STP que resulta equivalente al protocolo clásico 802.1D y es llamado **PVST+** (Per-VLAN Spanning Tree Plus)

![[Pasted image 20241110205531.png|500]]

## STP algorithm and port roles 
El proceso STP para crear una loop-free topology es llamada [[STP algorithm]], que consiste fundamentalmente en tres pasos. 
1. Root bridge election 
2. Root port selection 
3. Designated port selection 

Este algoritmo termina por definir los roles que cumplen cada puerto y switch. Se genera un _root_, _designated_ y _non-designated_ ports, tambien llamados **STP port roles**. 


> Los BPDUs son enviados a los switches STP-enabled usando direcciones MAC address multicast, dependiendo el protocolo STP que se este ejecutando.
> - Para PVST+ / RPVST+ se usa la multicast adddress 0100.0CCC.CCCD
> - Para IEEE 802.d / 802.1w se usa la multicast address 0180.C200.0000
## STP port states and times 
En el proceso [[Project/Networking/CCNA-notas/Spanning Tree Protocol/STP|STP]], tambien tenemos los llamados **port states**, en la definición de [[STP algorithm]] se cubrieron el _forwarding_ state y el _blocking_ state. 

Ademas de estos estados tambien se encuentra estados intermedios que preparan el forwarding de frames, asi como timers que rigen la cantidad de tiempo que pasa un puerto en determinado estado. 

- [[STP port states]] 
- [[STP timers]]

## PortFast and BPDU Guard 
Estos son añadidos que agrega Cisco para acelerar la STP convergencia y asegurar la estabilidad.

> El proceso STP actua sobre todos los puertos de un switch, incluso los inactivos. Los hosts finales como PCs, son siempre asignados como _designated ports_.

El problema con los hosts finales es que deberian esperar 30 segundos (entre listening y learning states) para finalmente llegar al forwarding state y poder conectarse a la red. En estos casos se acuden a las siguientes caracteristicas. 
- [[PortFast]]  
- [[BPDU Guard]] 


