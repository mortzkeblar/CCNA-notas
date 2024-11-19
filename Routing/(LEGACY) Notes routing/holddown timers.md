---
tags:
  - routing
  - dynamic
  - CCNA
---

Este método es usado para prevenir el envió/recepción de mensajes de actualización en un periodo de tiempo, conocido como _holddown time_, previniendo así p. ej. agregar una ruta que en realidad esta caída. 
A veces, antes que la red falle completamente, suele tener un up y down rápido, a esto se le conoce como _link flapping_, el router es consciente de esto e informa el estado de la red según su estado hasta que esta down de forma prolongada. 

Cuando _holddown timer_ esta en uso, el router activa el temporizador cuando ve que la interface es down, si la interface vuelve a estar activa. Ignora la actualización y no realiza cambios, la teoría es que en ese tiempo la interface caída ya debiera estar up. 

 > holddown timers prevent routing table updates for X seconds

Algo que pueda ocurrir mientras este activo el _holddown timer_ es que el router reciba una actualización con una [metric]((OLD)%20metric.md) mejor desde un router diferente. Esta ruta sera agregada una vez que termine el tiempo de espera. El tiempo de espera puede modificarse según sea necesario (no recomendable).
- Valores muy altos provocan que el tiempo de convergencia sea más lento.
- Valores muy bajos producen una rapida convergencia pero puede ser ineficaz para casos de _link flapping_.

``` bash
_R1#show ip protocols_
_Routing Protocol is “rip”_
_Outgoing update filter list for all interfaces is not set_
_Incoming update filter list for all interfaces is not set_
_Sending updates every 30 seconds, next due in 15 seconds_
_Invalid after 180 seconds, **hold down 180**, flushed after 240_
_Redistributing: rip_
```
