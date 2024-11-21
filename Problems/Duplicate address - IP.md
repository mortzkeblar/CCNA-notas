---
tags:
  - problem
  - CCNA
date created: Saturday, October 19th 2024, 11:08:14 pm
date modified: Wednesday, November 20th 2024, 11:33:50 pm
---

``` bash
*Jan 12 13:22:14.276: %IP-4-DUPADDR: Duplicate address 192.168.40.1 on Vlan40, sourced by 0cf7.3  
```

Esto se debe a que tienes un IP duplicada dentro de la red, para poder ver de que interface proviene esa IP usando la MAC address 
``` bash
SW1(config)$ show mac address-table
          Mac Address Table
-------------------------------------------

Vlan    Mac Address       Type        Ports
----    -----------       --------    -----
   1    0cd3.1eb7.0000    DYNAMIC     Gi0/0

Total Mac Addresses for this criterion: 2

# Tambien podemos filtar la MAC especifica que estamos buscando, p. ej
SWR1(config)$ show mac address-table | include 0cf7
  40    0cf7.3967.0000    DYNAMIC     Gi0/3

```


Podemos ver que la MAC llega desde Gi0/3, tenemos que ver si la salida lo dirige hacia un host o hacia otro switch.

Si la salida lo dirige hacia otro switch, ejecute el mismo procedimiento en los dispositivos que sea necesario hasta encontrar al host culpable (no necesiamente es un host final, puede ser tambien un switch intermedio mal configurado).

![](_anexos_/EIFbb0nX4AALhvA%201.jpg)
_IP mala! Asustas a Smithers!_