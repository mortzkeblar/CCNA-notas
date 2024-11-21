---
tags:
  - ETHERCHANNEL
  - CCNA
date created: Saturday, October 19th 2024, 11:08:14 pm
date modified: Wednesday, November 20th 2024, 11:33:50 pm
---

![](Screenshot%20from%202024-01-04%2018-28-35.png)
``` bash

SW1(config)$ interface range g0/2-3
SW1(config-if-range)$ channel-group 1 mode desirable    
SW1(config-if-range)$ exit
SW1(config)$ interface Port-Channel 1
SW1(config-if)$ no switchport
SW1(config-if)$ ip address 192.168.0.1 255.255.255.252

SW2(config)$ interface range g0/2-3
SW2(config-if-range)$ channel-group 1 mode desirable
SW2(config-if-range)$ exit
SW2(config)$ interface Port-Channel 1
SW2(config-if)$ no switchport
SW2(config-if)$ ip address 192.168.0.2 255.255.255.252

```

Esto permite tener un puerto de router que tiene capacidad de ancho de banda superiores a las de un solo enlace.