---
tags:
  - routing
  - static
  - CCNA
date created: Saturday, October 19th 2024, 11:08:14 pm
date modified: Wednesday, November 20th 2024, 11:33:50 pm
---

Esta es una forma de usar una IP prestada de otra interface, esto puede ser util para guardar direcciones o para asegurar que la interface esta activa. 

A menudo se usa este comando con interfaces Loopback para incrementar la fiabilidad porque una interface Loopback no puede caer (solo existe a nivel de software). 

``` bash
R1#config t
R1#(config)#interface FastEthernet0/0
R1#(config-if)ip address 192.168.1.1 255.255.255.0
R1#(config-if)no shutdown
R1#(config-if)#interface Serial0/0
R1#(config-if)#ip unnumbered FastEthernet0/0
R1#(config-if)#no shutdown
```
