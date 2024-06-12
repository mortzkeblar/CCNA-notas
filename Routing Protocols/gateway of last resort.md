---
tags:
  - routing
  - static
---
Gateway of last resort es un término que describe la capacidad de un host/device para enviar todo el tráfico L3 que no tiene un destino/ruta especificado dentro de la [[routing table]]. Sin esta configuración, el paquete seria descartado. 

Esto puede habilitarse a través de [IP default-gateway (cisco command)](IP%20default-gateway%20(cisco%20command).md)  o bien enrutandolo hacia `0.0.0.0/0`.

``` bash
ip default-gateway 
ip route 0.0.0.0 0.0.0.0 <IP gateway>
```



