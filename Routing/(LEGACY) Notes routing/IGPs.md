---
tags:
  - CCNA
---
# Interior Gateway Protocols (IGPs)

_Este protocolo forma parte de los metodos de enrutamiento dinamico. Ver: [dynamic routing protocols]((LEGACY)%20Notes%20routing/dynamic%20routing%20protocols.md)_

![](Screenshot%20from%202024-01-05%2009-27-06.png)

Notas:
- RIPv1 y IGRP estan obsoletos (deprecated)
- IS-IS es un tema que ya no esta en el CCNA/CCNP 
- Actualmente, las opciones más sensatas (en empresas) a usar estan entre EIGRP o OSPF

> Pregunta: 
> Cual es la diferencia entre un protocolo _Distance Vector_ (Vector de distancia) contra un _Link State (Estado de enlace)?_
> En terminos simples los protocolos _Distance vector_ limitan el alcanze de visión de un dispositivo a las dispositivos vecinos a la que esta conectada. 
> En protocolos _Link state_ el router  tiene la lista completa de las rutas en la topologia de red. 


![](Screenshot%20from%202024-01-05%2010-02-22.png)
- Clase
	- DV: Distance vector
	- LS: Link state
	- PV: Path vector
- AD: es un valor que usan los routers para elegir la mejor ruta en base al protocolo usado (*Ver: [Metric and AD](Metric%20and%20AD.md)*)
- Metrica
	- BW: bandwidth (ancho de banda)
- Actualización de enrutamiento
	- Triggered: Se envia solo cuando hay un evento. No es periodico


## IGPs y EGPs
_Ver: [IGPs y EGPs](IGPs%20y%20EGPs.md)_
