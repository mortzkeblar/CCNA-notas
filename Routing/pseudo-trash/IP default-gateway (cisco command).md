---
tags:
  - routing
  - static
  - CCNA
---

Este comando solo se utiliza en routers sin [IP routing](IP%20routing.md) habilitado. En ese caso, el router solo actua como un  host que necesita un default-gateway. 
> #exam Debido a que es raro usar este comando, hay que tener cuidado con las preguntas capciosas del examen

En un contexto de L2, cuando se configura un [switch](../../VLAN/switch.md) se utiliza este comando para que se pueda gestionar de forma remota mediante telnet, y que el trafico pueda ser enviado al router en un caso de enrutamiento InterVLAN.

En un contexto de L3, default-gateway es el destino al que se reenvia el trafico cuando la _destination source_ no se encuentra dentro de la [[routing table]]. En ese sentido, es equivalente al concepto de [[gateway of last resort]]. 



