---
tags:
  - routing
  - dynamic
  - RIP
  - protocol
  - CCNA
date created: Saturday, October 19th 2024, 11:08:14 pm
date modified: Wednesday, November 20th 2024, 11:33:50 pm
---

> #exam Tener en cuenta que RIP ya no es un tema a examinar en el CCNA 200-301, sin embargo, no se lo va a obviar porque es una puerta introductoria para protocolos más complejos que continúan después y si están dentro del examen de certificación. 

Routing Information Protocol (RIP) esta basado en el algoritmo _Bellman-Ford_, desarrollado por Xerox. Fue diseñado para usarse en redes de pequeña o mediana escala.

RIPv1 al ser un protocol [classful](classful.md) , no reconoce [VLSM](VLSM.md). Por lo cual no puede interpretar la información respecto a subnet mask.   

Todas la rutas conocidas por RIP son enviadas mediante broadcast fuera de las interfaces RIP cada 30 segundos (y que no pasen la frontera de 15 hops de [maximum hop count](maximum%20hop%20count.md), ni [split horizon](split%20horizon.md)), incluyendo routers no configurados en RIP. 

Su única [[Dynamic Routing#Metric parameter]]  consiste en el _hop count_, por ende, no considera ni la velocidad o la confiabilidad del enlace. 

La [[Dynamic Routing#Administrative Distance parameter]] de RIP es de 120 (tanto para RIPv1 como [RIPv2](RIPv2.md)). 
