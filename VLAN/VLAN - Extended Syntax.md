---
tags:
  - VLAN
  - CCNA
---


``` bash
S1(config)$ do show vtp status
...
Mode: Server
...

S1(config)$ vpt mode transparent
S1(config)$ vlan 3000
S1(config-vlan)$ name INVITADOS
S1(config-vlan)$ exit

S1(config)$ do show vlan brief
```