---
tags:
  - routing
  - dynamic
  - RIP
  - CCNA
date created: Saturday, October 19th 2024, 11:08:14 pm
date modified: Wednesday, November 20th 2024, 11:33:50 pm
---

Por defecto, [RIP](RIP.md) realiza load balancing sobre cuatro rutas de igual coste. Esto ayuda a no saturar un solo enlace cuando tenemos grandes cargas. 

Considerar que los enlaces deben ser iguales para que el load balancing sea equitativo. 
- En la primer etapa, el router elegirá la ruta con la [(legacy)administrative distance]((legacy)administrative%20distance.md) más baja. 
- Si hay varias coincidencias, usa la [metric]((OLD)%20metric.md) más baja. 
- Si aun existe empate entre routers, agregara esas rutas (hasta el maximo de rutas) en la tabla de enrutamiento.

Se puede cambiar el valor por defecto en hasta 16 como maximo (verificar en cada plataforma).

``` bash
R1(config)#router rip
R1(config-router)#maximum-paths ?
<1-16> Number of paths
```