_Rapid Spanning Tree Protocol (RSTP)_, es un protocolo de red definido en el IEEE 802.1w como una mejora al protocol [[Project/Networking/CCNA-notas/Spanning Tree Protocol/STP|STP]] IEEE 802.1d. 

> La versión RSTP propietaria de Cisco es **Rapid Per-VLAN Spanning Tree Plus (Rapid PVST+)** y es la que se usa en los [[switch]]es Cisco por defecto. La diferencia radica en que la versión propietaria genera un spanning tree diferente por cada [[VLAN]]. 

Para ver la versión de STP que corre un switch se puede usar `show spanning-tree`. Para elegir la versión de STP a usar se puede ejecutar `spanning-tree mode {pvst | rapid-pvst}`.

![[Pasted image 20241115231307.png]]

La desventaja tanto de las versiones propietarias de Cisco (tanto _PVST+_ como _Rapid PVST+_) es que en un entorno con una gran cantidad de [[VLAN]]s tambien se ejecutan varias instancias de [[Project/Networking/CCNA-notas/Spanning Tree Protocol/STP|STP]], lo que puede causar problemas de rendimiento.

Para solventar este problema es preferible usar **Multiple Spanning Tree Protocol (MSTP)**, esto permite agrupar varias VLANs dentro de una unica instancia, además usa el macanismo de RSTP para una rapida convergencia. 

![[Pasted image 20241115232044.png|400]]

**STP versions**

![[Pasted image 20241115232121.png]]

## RSTP algorithm

> La mayor ventaja de RSTP frente a STP, es que mientras [[Project/Networking/CCNA-notas/Spanning Tree Protocol/STP|STP]] es un protocolo basado en timers, RSTP usa un mecanismo de sincronización en el cual los dispositivos pueden cambiar de estado inmediatamente. Sin necesidad de esperar a un contador como se hace en STP. 

El algoritmo usado para calcular la topologia en RSTP es identica a la usada en [[Project/Networking/CCNA-notas/Spanning Tree Protocol/STP|STP]]. A saber:
- Root bridge election (una por LAN)
	- Lowest bridge ID (BID)
- Root port selection (one per switch, excluding root bridge)
	- Lowest root cost 
	- Lowest neighbor BID 
	- Lowest neighbor port ID 
- Designated port selection (one per segment)
	- Port on switch with lowest root cost 
	- Port on switch with lowest BID 