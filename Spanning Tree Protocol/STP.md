_Spannign Tree Protocol (STP)_, es un protocolo (habilitado por defecto en [[switch]]es Cisco) que ayuda a resolver problemas de tipo _Layer 2 loops_ dentro de una [[LAN]], estos son frames que estan en loop dentro de la red de forma indefinida. 

> El problema que ataca STP es similar a [[TTL]] en la [[IPv4 Header]]. 

Este problema suele aparecer principalmente cuando se trata de introducir conexiones redundantes en la red. Se puede identificar dos problemas principales: _broadcast storn_ y _MAC address flapping_.

![[Pasted image 20241110201000.png]]

- *Broadcast storn* - se conoce como tormenta broadcast cuando la cantidad de frames que hacen loop en la red. Comienzan a saturar al punto de consumir muchos recursos como CPU o ancho de banda haciendo que la red se inutilizable.
- *MAC address flapping* - este problema ocurre cuando un [[switch]] aprende la misma MAC address desde dos puertos diferentes. 

Spannign Tree Protocol se puede resumir como _la prevención de Layer 2 Loops mediante el bloqueo de la conexiones redundantes  a nivel de  la topologia LÓGICA (no FÍSICA), manteniendo una unica ruta activa entre dos nodos cualquieras en un una [[LAN]]_. 

![[Pasted image 20241110205531.png|500]]

## STP algorithm 
Este es el proceso para crear un topologia loop-free, los pasos consisten en:
- Root bridge election
- Root port selection 
- Designated port selection 

![[Pasted image 20241110213336.png|500]]

### Root bridge election 

Primeramente STP elige un [[switch]] para como _root bridge_ para la LAN. Un root bridge es el punto central de referencia para la topología STP. 

> Todos los switches en la red tienen garantizado una ruta activa hacia el root bridge 

> En STP se puede tomar como equivalentes _bridge_ y _switch_.

- La elección del _root bridge_ se lleva a cabo con los switches que comparten el _STP Bridge Protocol Data Unit (BPDU)_ entre sí
	- Los BPDU se usa para tomar todas las decisiones en el STP algorithm, no solo para la elección del root bridge 
- Los mensajes BPDU se envian cada 2 segundos, entre los datos enviados. Los campos importantes para la elección del root bridge son 
	- My _Bridge Identifier (BID)_, el número de identificación unico propio del switch que envia el BPDU en la [[LAN]]
	- El _BID_ del switch que creen que es el root bridge 
		- Al iniciar un [[switch]], todos se identifican asi mismos como el root bridge
			- Esto significa que el [[switch]] al iniciarse, pone su propio BID en el campo del BID del que cree que es el root bridge

#### Bridge Identifier (BID)
El switch con el BID superior es el elegido como root bridge de la [[LAN]], este criterio se basa en los parametros que considere el STP algorithm. 
- El bridge root elegido implica que envio el BPDU con el campo _My BID_ numericamente más bajo

El BID (o Bridge ID) es un número de 64 bits que consiste de 16 bits como _bridge priority_ y los restantes 48 bits como la _MAC address_.
- _Bridge priority_ - esta se divide en dos partes, el valor del bridge priority es la suma de los dos campos que la componen. $Bridge\ priority=Priority + Extended\ System\ ID\ (VLANID)$
	- _Priority_, por defecto tiene el bit más significativo en 1 (esto equivale al valor 0d32768).
	- _Extended System ID (VLAN ID)_, el valor de este campo es igual al de la VLAN ID 

![[Pasted image 20241110225637.png]]

Los switches Cisco corren una versión propietaria de STP llamada **Per-VLAN Spanning Tree Plus (PVST+)**. PVST+ crea diferentes instancias de STP por cada VLAN, esto es util ya que se puede realizar un balanceo en el trafico, ya que se puede elegir los enlaces a deshabilitar según la VLAN.

![[Pasted image 20241110232819.png|400]]

> Como la VLAN ID forma parte del BID, es significa que el campo _bridge priority_ es unico para cada instancia STP. 







