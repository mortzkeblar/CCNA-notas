---
tags:
  - routing
  - dynamic
  - RIP
---

Tanto RIPv1 como [RIPv2](RIPv2.md)  tiene operaciones y timers para el el control de actualizaciones de routing. Abajo esta la lista ordenada por defecto de operaciones al iniciar RIP, los timers estan disponibles a través del comando `show ip protocols`.
- Initialization - se envia una petisión (request) desde la interface preguntando por la tabla de table de enrutamiento completa a todos los routers RIP. 
- Request received - se devuelve al solicitante la tabla de enrutamiento como mensaje response-received. 
- Response received - Este mensaje indica que la tabla de enrutamiento ha sido actualizada
- Routing/Timers - estos son inválidos despues de 180 segundos, se actualizan las tablas de enrutamiento cada 30 segundos. Cuando pasan los 180 segundos se considera unreachable (estableciendo 16 hops segun [maximum hop count](maximum%20hop%20count.md)) y se vacian despues de 240 segundos. Cisco usa como timer para RIP [holddown timers](holddown%20timers.md). 
- [triggered updates](triggered%20updates.md) - se envían si cambia la [metrics](metrics.md) de la ruta, no se actualiza toda la tabla, solo los cambios. 

Los timers se puede cambiar con el comando `timers`, se puede modificar el update invalid, holddown y flush (en ese orden).

``` bash
R1(config-router)#timers basic ?
<0-4294967295> Interval between updates
R1(config-router)#timers basic 20 ?
<1-4294967295> Invalid
R1(config-router)#timers basic 20 160
```