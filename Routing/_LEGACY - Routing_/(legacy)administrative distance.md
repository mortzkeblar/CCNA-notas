---
tags:
  - routing
  - dynamic
  - CCNA
date created: Saturday, October 19th 2024, 11:08:14 pm
date modified: Wednesday, November 20th 2024, 11:33:50 pm
---

Esto es usado por el router para determinar las rutas más _fiables_ dependiendo el protocolo de routing. Generalmente siempre se prefiere a los protocolos con el AD más bajo. 
![](Screenshot%20from%202023-12-27%2016-43-38.png)

### Valores importantes (examen) sobre la distancia por defecto
> #exam _Este punto es para remarcar estos valores por su importancia para el examen de certificación_

| Route Source                       | Default Distance |
| ---------------------------------- | ---------------- |
| Connected interface                | 0                |
| Static route out an interface      | 0                |
| Static route to a next-hop address | 1                |
| EIGRP summary route                | 5                |
| Internel EIGRP                     | 90               |
| OSPF                               | 110              |
| RIPv1, RIPv2                       | 120              |
| External EIGRP                     | 170              |
| Unknown                            | 255              |
