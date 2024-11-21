---
tags:
  - ETHERCHANNEL
  - CCNA
date created: Saturday, October 19th 2024, 11:08:14 pm
date modified: Wednesday, November 20th 2024, 11:33:50 pm
---

Ambos extremos deben ser configurados manualmente y no existe ningun protocolo de autonegociación.

![](Screenshot%20from%202024-01-04%2017-12-19.png)

``` bash
SW1(config)$ interface range g0/2-3
SW1(config-if-range)$ channel-group 1 mode on      # Port channel 1
SW1(config-if-range)$ exit

SW1(config)$ interface range g0/2-3
SW1(config-if-range)$ channel-group 1 mode on      # No debe ser necesariamente el mismo numero
SW1(config-if-range)$ exit
```