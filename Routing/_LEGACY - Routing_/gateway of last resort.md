---
tags:
  - routing
  - static
  - CCNA
date created: Saturday, October 19th 2024, 11:08:14 pm
date modified: Wednesday, November 20th 2024, 11:33:50 pm
---
Gateway of last resort es un término que describe la capacidad de un host/device para enviar todo el tráfico L3 que no tiene un destino/ruta especificado dentro de la [[Routing Table]]. Sin esta configuración, el paquete seria descartado. 

Esto puede habilitarse a través de [IP default-gateway (cisco command)](IP%20default-gateway%20(cisco%20command).md)  o bien enrutandolo hacia `0.0.0.0/0`.

``` bash
ip default-gateway 
ip route 0.0.0.0 0.0.0.0 <IP gateway>
```



