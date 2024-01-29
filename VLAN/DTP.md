---
tags:
  - VLAN
  - concept
---

# Dynamic Trunking Protocol
DPT es un protocolo propietario de Cisco que permite a los switches Cisco a determinar de forma dinámica el estado de sus interfaces (ya sea `trunk` o `access`) sin una intervención manual. 
- Si DTP detecta otro switch en la conexión, forma un `trunk port`, de lo contrario, automaticamente lo convierte en un `access port`. 
- DTP viene habilitado por default en todas las interfaces de switches Cisco
- Por motivos de seguridad es recomendable desahabilitar DTP en todos los puertos de un switch

``` bash
Switch(config-if)#switchport mode ?
  access        Set trunking mode to ACCESS unconditionally
  dot1q-tunnel  set trunking mode to TUNNEL unconditionally
  dynamic       Set trunking mode to dynamically negotiate access or trunk mode
  private-vlan  Set private-vlan mode
  trunk         Set trunking mode to TRUNK unconditionally
```

``` bash
Switch(config-if)#switchport mode dynamic ?
  auto       Set trunking mode dynamic negotiation parameter to AUTO
  desirable  Set trunking mode dynamic negotiation parameter to DESIRABLE

```

#TODO continuar en:
https://www.youtube.com/watch?v=JtQV_0Sjszg&list=PLxbwE86jKRgMpuZuLBivzlM8s2Dk5lXBQ&index=35