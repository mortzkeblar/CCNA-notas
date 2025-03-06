---
tags:
  - routing
  - dynamic
  - RIP
  - CCNA
date created: Saturday, October 19th 2024, 11:08:14 pm
date modified: Wednesday, November 20th 2024, 11:33:50 pm
---


_Ver: [TTL](../../IPv4%20addressing/TTL.md) _
Esto es una función de [RIPv2](../RIP/RIPv2.md)   el cual te limita la cantidad de saltos a 15. Si un paquete llega a superar los 15 hops, este se descarta. 
Esto tiene un inconveniente y es que solo podemos usar RIP en redes que tengan como maximo, 15 saltos de distancia entre cada router.

Esto soluciona el [counting to infinity](counting%20to%20infinity.md)  pero no los [routing loops](routing%20loops.md) , porque siempre puede volver a comenzar de nuevo.
