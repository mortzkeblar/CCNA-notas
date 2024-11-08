---
tags:
  - CCNA
---
Una ruta conectada es una ruta a la red a la que esta conectada una interface. Esta se añade automaticamente a la [[routing table]] para cada interface que tenga configurada una IP address y este en activo. 

> i.e. Interface G0/0 de R1 tiene la IP address 192.168.0.1/24, entonces el router agrega automaticamente una ruta a la red 192.168.0.0/24 dentro de la [[routing table]].

Al estar conectada directamente, el router proporciona la interface a la que esta conectada. Para filtrar solo las rutas conectadas directamente se puede usar `show ip route | include C`.

> Una ruta a más de una dirección IP de destino es llamada una _network route_, como tal se trata de una ruta hacia una red en lugar de a una sola IP address. Un [[connected route]] es un tipo de network route.  
