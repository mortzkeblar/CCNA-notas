---
tags:
  - routing
  - dynamic
  - CCNA
date created: Saturday, October 19th 2024, 11:08:14 pm
date modified: Wednesday, November 20th 2024, 11:33:50 pm
---

Tambien llamados _shortest-path-first protocols_, los routers intercambian información entre ellos sobre sus _link states_. Cada dispositivo tiene un mapa de toda la red, construido a partir de la información que otros routers generan y propagan. 

A diferencia de los protocolos _route-by-rumor_ como [(legacy) distance vector protocols]((legacy)%20distance%20vector%20protocols.md), en el que solo se intercambian las mejores rutas entre vecinos. En link state protocol los dispositivos inundan TODA LA RED con información acerca de sus enlaces, esto permite que cada router tenga un conocimiento detallado de la red completa. 

La decisiones de routing se toman en base al _Shortest Path First (SPF)_, o al _Dijkstra's algorithm_, con el fin de calcular el camino más corto (shortest path) para llegar al destino. 

### link state routing protocol behavior
Los link state protocol actualizan la información de enrutamiento cada 30 minutos (en [(legacy) distance vector protocols]((legacy)%20distance%20vector%20protocols.md) eran cada 30 segundos en promedio, [RIPv2](../RIP/RIPv2.md) p. ej.), no existe un temporizador con intervalos regulares como [holddown timers](holddown%20timers.md). 
Esto tiene grandes ventajas frente a [(legacy) distance vector protocols]((legacy)%20distance%20vector%20protocols.md) porque:
- La información enviada es solo una actualización con los cambios especificos
- Una vez los cambios llegan a toda la red, no se vuelve a intercambiar información. Lo cual reduce el ancho de banda al no enviar actualizaciones innecesarias. 

Algunos ejemplos conocidos de link state protocol son OSPF (Open Shortest Path First), descrito en el RFC 2328 , e IS-IS (Intermediate System to Intermediate System), descrito en RFC 1142.

