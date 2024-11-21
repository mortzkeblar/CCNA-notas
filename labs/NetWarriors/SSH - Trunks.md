---
tags:
  - general
  - VLAN
  - CCNA
date created: Saturday, October 19th 2024, 11:08:14 pm
date modified: Wednesday, November 20th 2024, 11:33:50 pm
---

#### VLAN  - Trunks

Para poder usar VLAN entre switches es necesario configurar enlaces troncales (trunks)
``` bash
# trunk
Switch(config)$ int g0/1
Switch(config-if)$ switchport mode trunk
Switch(config-if)$ switchport trunk allowed vlan all
Switch(config-if)$ 


# Configuracion en el switch MLS
Switch(config)$ int g0/1
Switch(config-if)$ switchport trunk encapsulation dot1q
Switch(config-if)$ sw mode trunk
Switch(config-if)$ sw trunk allowed vlan all

```

Debido a que son diferentes switches es necesario configurar dot1q en ambos switches. 
Si ambos switches fueran de L2, sola haria falta la configuracion de un solo lado.

``` bash
Switch(config)$ ip domain cisco.com
Switch(config)$ crypto key generate rsa
Switch(config)$ username michely password 12345
Switch(config)$ line vty 0 15 
Switch(config-line)$ transport input ssh
Switch(config-line)$ login local
Switch(config)$ ip ssh version 2
```

- SSH se debe configurar en cada equipo, cada switch debe tener su IP address configurado