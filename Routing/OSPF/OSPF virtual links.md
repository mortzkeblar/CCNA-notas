---
tags:
  - routing
  - dynamic
  - OSPF
  - CCNA
date created: Saturday, October 19th 2024, 11:08:14 pm
date modified: Wednesday, November 20th 2024, 11:33:50 pm
---

Los virtual link son usados para extender el Area 0 a través de otras areas y cumplen la regla que todas las areas non-zero deben conectarse directamente al Area 0 (backbone). Un virtual link es usado para LSAs tunneling en un area non-zero. 

![](15-3.jpg)

Estos se basan en IDs de routers fijos porque el valor de RID es usado en la configuración del virtual link.

> Cisco advierte que el uso de virtual links indica un mal diseño de la red [OSPF](OSPF.md) 