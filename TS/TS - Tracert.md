---
tags:
  - TS
  - example
---

### Traceroute 

![](_anexos_/Screenshot%20from%202024-01-02%2003-03-29.png)

- A medida que un paquete este viajando en la red, el TTL va disminuyendo.
- El primer mensaje`traceroute` al saltar al siguiente router, el TTL vale 0
		- Esto con la intensión de que el router devuelve el mensaje, confirmando asi su existencia
- Una vez hecho esto aumenta +1, para que pueda seguir avanzando en la red

> Dicho: se dice que el diametro de internet esta en torno a los 30 routers

``` bash
$ traceroute <IP_TARGET>

$ traceroute -w 1 <IP_TARGET>

# El parametro -I indica de enviar mensajes ICMP
$ traceroute -w 1 -I <IP_TARGET>
```

#### Extended Traceroute

![](_anexos_/Screenshot%20from%202024-01-02%2003-23-24.png)

