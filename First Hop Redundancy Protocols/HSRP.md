---
tags:
  - CCNA
---
**Hot Standby Router Protocol (HSRP)** es un protocolo propietario de Cisco de tipo [[FHRP]]. 
- Este usa los terminos _active_ / _standby_ para los roles que asumen los routers que usan el protocolo. 
- _Preemption_ - significa que el router que fue activo y cambio de rol por algún motivo, este puede reclamar nuevamente su rol como activo. Deshabilitado pro defecto en HSRP. 
- Existen dos versiones de HSRP: v1 y v2
	- HSRPv1 
		- Envia mensajes multicast a la IP 224.0.0.2 
		- El formato de la Virtual MAC address es `0000.0c07.acXX`, donde `XX` es el número del grupo HSRP
		- Solo tiene soporte para IPv4
	- HSRPv2
		- Envia mensajes multicast a la IP 224.0.0.102 
		- El formato de la Virtual MAC address es `0000.0c9f.fXXX`, donde `XXX` es el número del grupo HSRP
		- Soporte para IPv4 y IPv6

Como se menciono en [[FHRP]], se puede elegir el _active_ / _standby_ router de forma independiente para cada subred, esto da la posibilidad de generar load balancing en los routers para evitar la congestión en un solo enlace. 

![[Pasted image 20241206001647.png]]

### Configuration 

![[Pasted image 20241206012613.png]]
![[Pasted image 20241206015833.png]]

- `standby <group-id> ip <virtual-ip>`
- `standby <group-id> priority <priority>` - la prioridad determina cual se designa como _active_ router, el valor por defecto es $100$. El active router puede ser elegido según: 
	- El router con el `priority` más alto 
	- El rotuer con la [[IP address]] más alta 
- `standby <group-id> preempt` - habilita _preemption_
	- Solo es necesario configurarlo en el router con el `priority` más alto, no necesitas configurarlo en todos los routers 

Para ver el estado de la configuración, se puede usar `show standby brief`.

![[Pasted image 20241206020656.png]]


