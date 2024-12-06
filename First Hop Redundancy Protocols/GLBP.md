**Gateway Load Balancing Protocol (GLBP)** es un protocol [[FHRP]] propietario de Cisco. 

Una caracterista unica de GLBP frente a [[Project/Networking/CCNA-notas/First Hop Redundancy Protocols/HSRP|HSRP]] o [[VRRP]] es que es el unico protocolo que proporciona load balancing dentro de unica subred (no confundir con el load balancing por subred), el balanceo se realiza *por host*. 

GLPB asigna un router como _Active Virtual Gateway (AVG)_ y puede asignar hasta cuatro routers como _Active Virtual Forwarders (AVF)_.
- Los AVF son routers activos que realizan el forwarding de paquetes, lo que permite realizar el load balancing de una subred por la cantidad de hosts 
- El AVG tambien puede ser un AVF 
- _Preemption_ 
	- En AVG deshabilitado por defecto 
	- En los AVF habilitado por defecto 

Si bien existe una sola _Virtual IP (VIP)_, el load balancing se logra asignando a cada AVF una unica _Virtual MAC address_. 
- El AVG se encarga de responder los [[ARP]] requests, pero en vez de responder el ARP request con su propia MAC address, lo hace con la MAC address de alguno de los AVF 
	- Estos respuestas se realizan utilizan _round-robin_, un metodo para distribuir tareas de forma cíclica. Lo que permite distribuir la carga que realizan los hosts de forma equilibrada por todos los AVFs disponibles.

![[Pasted image 20241206011141.png]]

- Al igual que [[Project/Networking/CCNA-notas/First Hop Redundancy Protocols/HSRP|HSRP]]v2, GLBP envia mensajes hello usando la IP multicast reservada 224.0.0.102. 
- El forma de la virtual MAC address es `0007.b400.XXYY`, donde `XX` representa el número de grupo GLBP y `YY` el número de AVF.