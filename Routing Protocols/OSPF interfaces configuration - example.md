---
tags:
  - routing
  - dynamic
  - OSPF
---

 > #exam Puede configurar alternativamente areas [OSPF](OSPF.md)  por interface, deberá estar familiarizado con ambos metodos para el examen (_Ver: [single area configuration ](single%20area%20configuration%20-%20example.md)_). 


![](_anexos_/15-7.jpg)

``` bash
# R1
R1(config)#int f0/0
R1(config-if)#ip ospf 1 area 0
R1(config-if)#end
Technet24.ir
*Mar 1 01:19:37.127: %OSPF-5-ADJCHG: Process 1, Nbr 192.168.1.2 on
FastEthernet0/0 from LOADING to FULL, Loading Done

# R2
R2(config)#int f0/0
R2(config-if)#ip ospf 2 area 0
R2(config-if)#end
*Mar 1 01:19:35.879: %OSPF-5-ADJCHG: Process 2, Nbr 192.168.1.1 on
FastEthernet0/0 from LOADING to FULL, Loading Done

R1#show ip protocols
Routing Protocol is “ospf 1”
Outgoing update filter list for all interfaces is not set
Incoming update filter list for all interfaces is not set
Router ID 192.168.1.1
Number of areas in this router is 1. 1 normal 0 stub 0 nssa
Maximum path: 4
Routing for Networks:
Routing on Interfaces Configured Explicitly (Area 0):
FastEthernet0/0
Reference bandwidth unit is 100 mbps
Routing Information Sources:
Gateway Distance Last Update
192.168.1.1 110 00:01:11
Distance: (default is 110)
R1#show ip ospf neighbor
Neighbor ID Pri State Dead Time Address Interface
192.168.1.2 1 FULL/DR 00:00:31 192.168.1.2 F0/0
R1#show ip ospf int f0/0
FastEthernet0/0 is up, line protocol is up
Internet Address 192.168.1.1/24, Area 0
Process ID 1, Router ID 192.168.1.1, Network Type BROADCAST, Cost:10
Enabled by interface config, including secondary ip addresses
Transmit Delay is 1 sec, State BDR, Priority 1
Designated Router (ID) 192.168.1.2, Interface address 192.168.1.2
Backup Designated Router (ID) 192.168.1.1, Interface address 192.168.1.1
Timer intervals configured, Hello 10, Dead 40, Wait 40, Retransmit 5
oob-resync timeout 40
Hello due in 00:00:09
[output truncated]
Neighbor Count is 1, Adjacent neighbor count is 1
Adjacent with neighbor 192.168.1.2 (Designated Router)
Suppress Hello for 0 neighbor(s)
```