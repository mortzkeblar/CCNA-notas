---
tags:
  - routing
  - dynamic
  - RIP
  - CCNA
date created: Saturday, October 19th 2024, 11:08:14 pm
date modified: Wednesday, November 20th 2024, 11:33:50 pm
---

Los [routing loops](routing%20loops.md)  puede provocar otro problema, conocido como _counting to infinity_, cuando un paquete viaja a través de la red con un IP al que nunca llega. Esto sucede normalmente porque la red esta deshabilitada, pero pero el router emisor no es conciente de ello. 

![](13-18-scaled.jpg)

Router D anuncia que puede alcanzar la red `192.168.2.0`. Cuando el enlace con esa red falla, router D envia la información correspondiente a los demás routers. Router A (otra vez router A xd), sin embargo, informa que tiene un ruta de 2 saltos a `192.168.2.0` en su tabla, esta información es enviada a router B  (B piensa que esta a 3 saltos entonces según los saltos de A), que a su vez, informa a router D sobre que una ruta de 3 saltos esta disponible (4 saltos en la perspectiva de D). D a su vez envia a C que tiene una ruta de 4 saltos (5 saltos desde la perspectiva C). C envia esa información a A que si bien puede ver que los saltos aumentaron, aun asi esa información es agregada en la tabla. Formando así un bucle "infinito" (o hasta que TTL llegue a 0). 