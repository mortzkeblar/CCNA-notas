---
tags:
  - routing
  - dynamic
  - OSPF
  - CCNA
date created: Saturday, October 19th 2024, 11:08:14 pm
date modified: Wednesday, November 20th 2024, 11:33:50 pm
---

Si hay más de un camino igual (mismo [OSPF cost](OSPF%20cost.md) y  [[Dynamic Routing#Administrative Distance parameter]] para llegar a una ruta, entonces, por defecto [OSPF](OSPF.md) hace load balance para el trafico sobre cuatro caminos. 

```
R1#show ip protocols
Routing Protocol is “ospf 1”
Router ID 10.0.0.1
Number of areas in this router is 1. 1 normal 0 stub 0 nssa
Maximum path: 4
Routing for Networks:
10.0.0.0 0.0.0.255 area 0
192.168.1.0 0.0.0.255 area 0
[output truncated]
```

Este valor puede aumentarse, es recomendable tener bien planificado el hacer esto. 

```
R1#conf t
Enter configuration commands, one per line. End with CNTL/Z.
R1(config)#router ospf 1
R1(config-router)#maximum-paths ?
<1-16> Number of paths
```