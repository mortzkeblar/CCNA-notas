---
tags:
  - routing
  - OSPF
  - CCNA
---

Este es una valor [OSPF](OSPF.md) asignado a un enlace para otro router. El costo se basa en el ancho de banda de un enlace, los routers Cisco calculan el costo como $108/bandwidth$ (redondeo hacia abajo). Se puede usar un bandwidth configurado o el que viene por defecto.

| Interface | Cost $108/bandwidth$ |
| ---- | ---- |
| ATM, Fast Ethernet, Gigabit Ethernet, 10/100 Gbps | 1 |
| HSSI (45 Mbps) | 2 |
| 16 Mbps Token Ring | 6 |
| 10 Mbps Ethernet | 10 |
| 4 Mbps Token Ring | 25 |
| T1 (1.544 Mbps) | 64 |
| DS-0 (64 K) | 1562 |
| 56 K | 1785 |


``` bash
# output for a Ethernet interface running at 10 Mbps

R1#show ip ospf int f0/0
FastEthernet0/0 is up, line protocol is up
Internet Address 192.168.1.1/24, Area 0
Process ID 1, Router ID 10.0.0.1, Network Type BROADCAST, Cost: 10
Transmit Delay is 1 sec, State DR, Priority 1

# Here is an interface running at 100 Mbps:

Router#show ip ospf int f0/0
FastEthernet0/0 is up, line protocol is up
Internet address is 192.168.1.1/24, Area 0
Process ID 1, Router ID 10.0.0.1, Network Type BROADCAST, **Cost: 1**
Transmit Delay is 1 sec, State BDR, Priority 1
```

Puedes modificar el costo manualmente con `ip ospf cont [1-65535] interface`. Como el costo es acumulativo, cada interface suma en el costo de toda la red. El costo se puede ver con el comando `show ip route`. El costo para la ruta de abajo es 11, mientras que 110 es la [(legacy)administrative distance]((legacy)administrative%20distance.md) para [OSPF](OSPF.md). 

``` bash
R1#show ip route
172.16.1.1 [110/**11**] via 192.168.1.2, 00:01:21, FastEthernet0/0
```

La siguiente salida muestra una interface `Loopback` con un costo de 1 y otra interface FastEthernet con un costo de 10, lo que resulta en un costo total de 11. 

``` bash
R2#show ip ospf interface
Loopback0 is up, line protocol is up
Internet Address 172.16.1.1/16, Area 0
Process ID 1, Router ID 192.168.1.2, Network Type LOOPBACK, **Cost: 1**
Loopback interface is treated as a stub Host
FastEthernet0/0 is up, line protocol is up
Internet Address 192.168.1.2/24, Area 0
Process ID 1, Router ID 192.168.1.2, Network Type BROADCAST, **Cost: 10**
Transmit Delay is 1 sec, State BDR, Priority 1
```