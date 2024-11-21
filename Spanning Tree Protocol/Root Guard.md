---
tags:
  - CCNA
  - SpanningTreeProtocol
date created: Sunday, November 17th 2024, 2:17:37 am
date modified: Wednesday, November 20th 2024, 11:33:50 pm
---
_Root Guard_ es una caracteristica para asegurar la estabilidad del STP topology al prevenir que switches o dispositivos externos puedan convertirse en _root bridge_. Se asegura de que la topologia se mantenga incluso si se conecta un switch con un _lower bridge ID_.

> Esto caso puede darse en situaciones de una LAN controlada por dos entidades diferentes como un SP y un cliente. 

![[Pasted image 20241116201241.png]]

Cuando un puerto Root Guard-enabled recibe un BPDU superior, el puerto entra en _root-inconsistent_ state. Lo que provoca que se desactive el puerto, para solucionar el problema el nuevo switch debe configurar el _bridge ID_ a un valor más alto que el valor del switch que esta bloqueando su ingreso, para que este le permita conectarse. 

Para habilitar Root Guard se usa el comando `spanning-tree guard root`.

![[Pasted image 20241116201849.png]]
