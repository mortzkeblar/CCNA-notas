---
tags:
  - routing
  - dynamic
  - OSPF
  - CCNA
date created: Saturday, October 19th 2024, 11:08:14 pm
date modified: Wednesday, November 20th 2024, 11:33:50 pm
---

Recordad que en [OSPF](OSPF.md) se puede utilizar `passive-interface default` para configurar una [passive interface](passive%20interface.md). 

``` bash
R2#show ip ospf interface
FastEthernet0/0 is up, line protocol is up
Internet Address 192.168.1.2/24, Area 0
Timer intervals configured, Hello 10, Dead 40, Wait 40, Retransmit 5
oob-resync timeout 40
Hello due in 00:00:07
R2(config)#router ospf 1
R2(config-router)#passive-interface fast0/0
R2(config-router)#^z
R2#show ip ospf interface f0/0
Technet24.ir
FastEthernet0/0 is up, line protocol is up
Internet Address 192.168.1.2/24, Area 0
Process ID 1, Router ID 192.168.1.2, Network Type BROADCAST, Cost: 10
No Hellos (Passive interface)
```