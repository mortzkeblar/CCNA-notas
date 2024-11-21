---
tags:
  - routing
  - dynamic
  - OSPF
  - lab
  - CCNA
date created: Saturday, October 19th 2024, 11:08:14 pm
date modified: Wednesday, November 20th 2024, 11:33:50 pm
---
Temas a tratar:
- [OSPF](OSPF.md) 

![](16-9-scaled.jpg)

## lab objetives
1. Configurar un dirección IP en las interfaces de los routers.
2. Configurar [OSPF](OSPF.md) segun la descripción de la imagen.
3. Verificar el correcto funcionamiento de OSPF multi-area.
4. Verificar la [Routing Table](Routing%20Table.md). 

## resolution

1. Configurar todas la [IP](../../labs/NetWarriors/IP.md) segun la topologia de la imagen. 

``` bash
RouterA(config)# interface s0/1/0
RouterA(config-if)#ip address 10.0.0.1 255.255.255.252
RouterA(config)# interface lo0
RouterA(config-if)#ip address 172.20.1.1 255.255.255.0
RouterB(config)# interface s0/1/0
RouterB(config-if)#ip address 10.0.0.2 255.255.255.252
RouterB(config)# interface lo0
RouterB(config-if)#ip address 192.168.1.1 255.255.255.192
```

2. Añadir OSPF al routers. 
	1. Para router A 
		1. Poner la red conectada a Loopback 0 en el area 1
		2. Poner la red `10` en el area 0
	2. Para router B 
		1. Poner la interface Serial en el area 0
		2. Poner la red conectada a Loopback 0 en el area 2

``` bash
# router A

RouterA(config)#router ospf 4
RouterA(config-router)#network 172.20.1.0 0.0.0.255 area 1
RouterA(config-router)#network 10.0.0.0 0.0.0.3 area 0
RouterA(config-router)#^Z
RouterA#
%SYS-5-CONFIG_I: Configured from console by console
RouterA#show ip protocols
Routing Protocol is ospf 4
Outgoing update filter list for all interfaces is not set
Incoming update filter list for all interfaces is not set
Router ID 172.20.1.1
Number of areas in this router is 2. 2 normal 0 stub 0 nssa
Maximum path: 4
Routing for Networks:
172.20.1.0 0.0.0.255 area 1
10.0.0.0 0.0.0.3 area 0
Routing Information Sources:
Gateway Distance Last Update
172.20.1.1 110 00:00:09
Distance: (default is 110)

# router B

RouterB(config)#router ospf 2
RouterB(config-router)#net 10.0.0.0 0.0.0.3 area 0
00:22:35: %OSPF-5-ADJCHG: Process 2, Nbr 172.20.1.1 on Serial0/1/0 from
LOADING to FULL, Loading Done
RouterB(config-router)#net 192.168.1.0 0.0.0.63 area 2
RouterB(config-router)# ^Z
RouterB#show ip protocols
Routing Protocol is ospf 2
Outgoing update filter list for all interfaces is not set
Incoming update filter list for all interfaces is not set
Router ID 192.168.1.1
Number of areas in this router is 2. 2 normal 0 stub 0 nssa
Maximum path: 4
Routing for Networks:
10.0.0.0 0.0.0.3 area 0
192.168.1.0 0.0.0.63 area 2
Routing Information Sources:
Gateway Distance Last Update
172.20.1.1 110 00:01:18
192.168.1.1 110 00:00:44
Distance: (default is 110)
```

3. Revisar la [Routing Table](Routing%20Table.md) en los routers. Deberiamos ver la red [OSPF](OSPF.md) anunciada, bajo _IA_ (que significa OSPF inter-area). 

``` bash
RouterA#sh ip route
[output truncated]
10.0.0.0/30 is subnetted, 1 subnets
C 10.0.0.0 is directly connected, Serial0/1/0
172.20.0.0/24 is subnetted, 1 subnets
C 172.20.1.0 is directly connected, Loopback0
192.168.1.0/32 is subnetted, 1 subnets
O IA 192.168.1.1 [110/65] via 10.0.0.2, 00:01:36, Serial0/1/0
RouterA#

## Tenemos la opción de usar cualquier de estos comandos 

RouterA#sh ip ospf ?
[1-65535] Process ID number
border-routers Border and Boundary Router Information
database Database summary
interface Interface information
neighbor Neighbor list
```


