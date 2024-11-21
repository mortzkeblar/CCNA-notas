---
tags:
  - CCNA
  - SpanningTreeProtocol
date created: Thursday, November 14th 2024, 3:02:42 am
date modified: Wednesday, November 20th 2024, 11:33:50 pm
---
_PortFast_ es una caracteristica [[Project/Networking/CCNA-notas/Spanning Tree Protocol/STP|STP]] opcional que permite a un puerto pasar inmediatamente a un _forwarding_ state. Esto permite a los hosts finales conectarse inmediatamente a la red. 

![[Pasted image 20241114025548.png]]

Para habilitar PortFast en un puerto especifico se puede usar `spanning-tree port-fast`. Para habilitar para todos los puertos (de acceso, no trunks) se puede usar el comando `spanning-tree portfast default`. 

![[Pasted image 20241114025909.png]]
> Es importante tener cuidado al usar PortFast, solo se use para hosts finales. Ya que al bypassear el _listening/learning_ states queda vulnerable ante _layer 2 loops_.