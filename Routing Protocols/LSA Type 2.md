---
tags:
  - dynamic
  - routing
  - OSPF
---
**Network Link State Advertisements**, es usado para anunciar routers en segmentos multi-access. Esta LSA es originada por el [DR](DR%20and%20BDR.md) y hacen flooding dentro del area, al igual que [LSA Type 1](LSA%20Type%201.md). 

El LSA Type 2 incluye el link ID (el cual es la dirección del DR), la mascara y los [RID](RID.md)s de los routers en la red (routers conectados). 

``` bash
R3#show ip ospf database network
OSPF Router with ID (3.3.3.3) (Process ID 3)
Net Link States (Area 0)
Routing Bit Set on this LSA
LS age: 248
Options: (No TOS-capability, DC)
LS Type: Network Links
Link State ID: 10.0.1.2 (address of Designated Router)
Advertising Router: 2.2.2.2
LS Seq Number: 80000008
Checksum: 0x8E7B
Length: 32
Network Mask: /24
Attached Router: 2.2.2.2
Attached Router: 3.3.3.3
```