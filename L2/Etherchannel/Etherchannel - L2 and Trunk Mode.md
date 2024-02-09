---
tags:
  - ETHERCHANNEL
  - configuration
---


Este modo suele ser el modo más comun de configuración de etherchannel.
![](_anexos_/Screenshot%20from%202024-01-04%2018-23-03.png)

``` bash

SW1(config)$ interface range g0/2-3
SW1(config-if-range)$ channel-group 1 mode active      
SW1(config-if-range)$ exit
SW1(config)$ interface Port-Channel 1
SW1(config-if)$ switchport trunk encapsulation dot1q
SW1(config-if)$ switchport mode trunk

SW2(config)$ interface range g0/2-3
SW2(config-if-range)$ channel-group 1 mode passive      
SW2(config-if-range)$ exit
SW2(config)$ interface Port-Channel 1
SW2(config-if)$ switchport trunk encapsulation dot1q
SW2(config-if)$ switchport mode trunk
```