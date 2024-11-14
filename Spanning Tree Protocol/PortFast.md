---
tags:
  - CCNA
  - Spanning-TreeProtocol
---
_PortFast_ es una caracteristica [[Project/Networking/CCNA-notas/Spanning Tree Protocol/STP|STP]] opcional que permite a un puerto pasar inmediatamente a un _forwarding_ state. Esto permite a los hosts finales conectarse inmediatamente a la red. 

![[Pasted image 20241114025548.png]]

Para habilitar PortFast en un puerto especifico se puede usar `spanning-tree port-fast`. Para habilitar para todos los puertos (de acceso, no trunks) se puede usar el comando `spanning-tree portfast default`. 

![[Pasted image 20241114025909.png]]
> Es importante tener cuidado al usar PortFast, solo se use para hosts finales. Ya que al bypassear el _listening/learning_ states queda vulnerable ante _layer 2 loops_.