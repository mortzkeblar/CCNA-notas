---
tags:
  - CCNA
  - SpanningTreeProtocol
date created: Thursday, November 14th 2024, 11:46:48 pm
date modified: Wednesday, November 20th 2024, 11:33:50 pm
---
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

![[Pasted image 20241116130515.png]]

A diferencia de [[Project/Networking/CCNA-notas/Spanning Tree Protocol/STP|STP]], en RSTP los puertos remanentes no terminan como _non-designated ports_, sino que cumplen uno o dos roles: **alternative** o **backup** ports.
Otra diferencia es que en RSTP, todos los switches  BPDUs por sus _designated_ ports, independiente de si recibio BPDUs del root bridge o no. 

### Port costs 
RSTP agrega un conjunto de costos para los puertos de velocidades mayores a 10Gbps.
- Los valores de costos definidos anteriormente en [[Project/Networking/CCNA-notas/Spanning Tree Protocol/STP|STP]] son llamados **short costs**
- Los valores de costos definidos en RSTP son llamados **long costs**

![[Pasted image 20241116124143.png]]

Por defecto (tanto en STP como RSTP) se usa el metodo _short cost_. Para ver la metodo utilizado se usa `show spanning-tree pathcost method`, para cambiar el metodo a usar se puede ejecutar `spanning-tree pathcost method {short | long}`.

### Port states 
![[Pasted image 20241116125421.png]]
En RSTP, se combinan _blocking_,  _listening_ y _disabled_ state en un solo estado: **discarding** state. 
- Cuando un puerto RSTP es habilitado por primera vez, entre en _discarding_ state. 
	- Si se decide que el puerto va a tener un rol de _alternative_ o _backup_ port, se mantiene en este estado. En la que se bloquea el trafico para evitar los _layer 2 loops_.
	- Si se decide que el puerto pasa a ser _root_ o _designated_ port, pasa a tener el estado _forwarding_. Esto puede ocurrir de dos formas. 
		- Si el mecanismo de RSTP sync es exitoso, el puerto pasa inmediatamente al forwarding state 
		- Si el mecanismo de RSTP sync falla, el puerto transiciona a el estado _learning_ para luego pasar al forwarding state. Como un [[Project/Networking/CCNA-notas/Spanning Tree Protocol/STP|STP]] tradicional. 
			- Los motivos para que RSTP sync falle ocurren principalmente si el otro extremo del enlace no soporta o no tiene habilitado RSTP. Esto suele concluir en que el enlace funciona como una instancia [[Project/Networking/CCNA-notas/Spanning Tree Protocol/STP|STP]] tradicional.

![[Pasted image 20241116132314.png]]

### Port roles 
- En RSTP, tanto el _root_ como _designated_ roles se mantiene como los descrito en [[STP algorithm]]. 
	- Root port - es un port forwarding que apunta hacia el root bridge, proporciona a los [[switch]]es non-root una unica ruta activa para llegar al root bridge 
	- Designated port - es un forwarding port que apunta hacia otros puntos fuera del root bridge, cada segmento tiene un unico designated port
- En RSTP el _non-designated_ port fue reemplazado por el _alternate_ y _backup_ port. En ambos casos, se bloquea el trafico en para prevenir los layer 2 loops. 

#### Alternate role 
Su función es de servir como alternativa para el _root port_ en caso de que este falle en el cual este puerto actua inmediatamente pasando al _forwarding_ state, proporciona otra ruta para llegar al root bridge.

Para que un puerto sea elegido como _alternate_ port: es cualquier puerto que no es elegido ni como root ni como designated port, además el [[switch]] no tiene que ser el _designated bridge_ para ese segmento. 

> Un **designated bridge** es un switch que tiene el _designated port_ para un segmento particular. Este termino aplica tanto a [[Project/Networking/CCNA-notas/Spanning Tree Protocol/STP|STP]] como a RSTP. 

![[Pasted image 20241116184857.png]]

Otra definición es que un alternate port es el discarding state que es conectado a un designated port de otro switch. 

#### Backup role 
Un RSTP _backup role_ es un puerto que sirve como una ruta de backup al mismo segmento como un _designated port_ en el mismo switch. Esto sucede si dos puertos en un mismo switch estan conectadas al mismo segmento [[collision domain]] (un hub i.e.)

Para que un puerto sea elegido como _backup_ port: cualquier puerto que no sea un _root_ ni un _designated_ port es un _backup_ port si el switch de ese puerto es un _designated bridge_ para ese segmento. 

![[Pasted image 20241116190334.png]]

Otra definición es que un backup port es el discarding state que esta conectado a un designated port en el mismo switch (via hub). 

![[Pasted image 20241116190600.png]]

En este caso que tenemos varios puertos conectados al mismo segmento, tanto el root port como el designated port son elegidos mediante un par de reglas adicionales que siguen desde de las ya definidas en [[STP algorithm]].
- Root port selection (one per switch, excluding root bridge)
	- Lowest local port ID 
- Designated port selection (one per segment)
	- Lowest local port ID 

### RSTP topology changes 
Como se vio en [[STP timers]], para que se genere la convergencia en la topologia despues de un cambio suele tardar 50 segundos hasta completar las operatorias. En RSTP esto se reduce a solo 6 segundos. 
 ![[Pasted image 20241116191651.png]]
En este caso, G0/1 de SW1 deja de recibir BPDUs (sin que el puerto caiga). SW1 G0/1 siguien siendo el root port durante 6 segundos (3x el hello timer de 2 segundos). Luego G0/0 pasa a ser el root port y despues de realizar el proceso RSTP sync, este cambia inmediatamente al forwarding state. 
- Si es que la falla hiciera que el G0/1 caiga (puerto deshabilitado) no esperaria 6 segundos sino que iniciaria inmediatamente el RSTP sync process, esto duraria menos de 1 segundo. 

### RSTP link types 
Los _link types_ influyen en como los puertos transicionan a través de los RSTP port states y como reaccionan a los cambios de la red. 
- _Point-to-Point_ - puerto full-duplex que usa el mecanismo RSTP sync para realizar un transición inmediata al forwarding state. Se puede configurar con `spanning-tree link-type point-to-point`
- _Shared_ - puerto half-duplex que no puede usar el mecanismo RSTP sync. Su transición de estados es igual a un puerto [[Project/Networking/CCNA-notas/Spanning Tree Protocol/STP|STP]] standard. Se puede configurar con `spanning-tree link-type shared`   
- _Edge_ - puertos conectados a hosts finales que pueden usar [[PortFast]] para transicionar inmediatamente al forwarding state, no necesitan usar el RSTP sync porque no hay riesgo de un layer 2 loop. Para habilitar un edge link type se puede usar `spanning-tree portfast`
	- Un puerto que no este configurado con [[PortFast]] debera esperar 30 segundos para pasar el forwarding state, como un puerto STP regular. 
	- Una de las caracteristicas más importantes de los edge ports en RSTP, es que son excluidos de los enlaces a considerar cuando se realiza un cambio en la topologia. Por lo que si se producen cambios en esos puertos RSTP no realiza el proceso de convergencia.  
	- Al ejecutar `show spanning-tree` si el puerto edge esta configurado como half-duplex aparece como `shr edge`, si esta configurado como full-duplex aparece como `p2p edge`

![[Pasted image 20241116193020.png]]

**Show type of links command**
![[Pasted image 20241116195724.png]]

### Root Guard, Loop Guard and BPDU Filter 
> Todas las funciones opcionales como [[PortFast]], [[BPDU Guard]], Root Guard, Loop Guard y BPDU Filter funcionan tanto en [[Project/Networking/CCNA-notas/Spanning Tree Protocol/STP|STP]] como en [[RSTP]]. 

- [[Root Guard]] 
- [[Loop Guard]] 
- [[BPDU Filter]] 