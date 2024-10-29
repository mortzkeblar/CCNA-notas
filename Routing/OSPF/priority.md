---
tags:
  - routing
  - dynamic
  - OSPF
  - CCNA
---
Si bien puede forzar la elección de un DR/BDR con `router-id` o setear una [IP](../../labs/NetWarriors/IP.md) Loopback alta. La forma recomendada es utilizar `ip ospf priority`. 

``` bash
R1(config)#int f0/0
R1(config-if)#ip ospf priority ?
	[0-255] Priority
```

Este comando no funciona si ya se eligio un DR/BDR. 

La prioridad por defecto es 1, si no quieres que la interface participe en la elección puede establecer el valor en 0. Si quieres que sea la DR, debes establecer un valor alto (p. ej. 200) y dejar las demás por defecto. 

``` bash
R1#show ip ospf int f0/0
FastEthernet0/0 is up, line protocol is up
Internet Address 192.168.1.1/24, Area 0
Process ID 1, Router ID 1.1.1.1, Network Type BROADCAST, Cost: 10
Transmit Delay is 1 sec, State DR, Priority 1
```

Puedes forzar la elección de un DR/BDR en un segmento con `clear ip ospf process`. No es recomendable hacer esto en producción a menos que sepa lo que esta haciendo. 

``` bash
R1#clear ip ospf process
Reset ALL OSPF processes? [no]: yes
*Mar 1 01:19:26.711: %OSPF-5-ADJCHG: Process 1, Nbr 192.168.1.2 on
FastEthernet0/0 from FULL to DOWN, Neighbor Down: Interface down or detached
*Mar 1 01:19:26.831: %OSPF-5-ADJCHG: Process 1, Nbr 192.168.1.2 on
FastEthernet0/0 from LOADING to FULL, Loading Done
```