---
tags:
  - routing
  - dynamic
  - RIP
  - CCNA
---

Los rutas por defecto, ahorran tiempo de administración al tener multiples routers finales que usan la misma ruta por defecto para el tráfico. La alternativa a esto es agregar [static route](static%20routing.md) en cada uno de los routers, lo cual puede ser tedioso y demorado. 

En una topologia mental, cualquier trafico no local va hacia R1 para ser reenviado a R2, y entonces hacia internet. Se hubiera configurado [static routing](static%20routing.md) en R3 y R4, pero el [default route]((OLD)%20default%20routes%20for%20RIPv2.md) se hace mucho más simple y sencillo de mantener. 

El comando `default-information originate` fue usado por el router gateway R2 para inyectar una ruta por defecto en la red [RIPv2](../RIP/RIPv2.md) .

``` bash
R3#show ip route
Gateway of last resort is not set
C    172.16.0.0/16 is directly connected, FastEthernet0/0
R    10.0.0.0/8 [120/1] via 172.16.0.1, 00:00:00, FastEthernet0/0
[output truncated]
```

Se configuro una [default route]((OLD)%20default%20routes%20for%20RIPv2.md) en R1, y se uso `default-information originate` para propagar hacia los otros  routers, la interface ethernet en R1 es `172.16.0.1`.
R3 ahora deberia ver el [gateway of last resort](pseudo-trash/gateway%20of%20last%20resort.md) e agregarla en la tabla de enrutamiento.

``` bash
R3#show ip route
Gateway of last resort is 172.16.0.1 to network 0.0.0.0
C    172.16.0.0/16 is directly connected, FastEthernet0/0
R    10.0.0.0/8 [120/1] via 172.16.0.1, 00:00:00, FastEthernet0/0
R   0.0.0.0/0 [120/1] via 172.16.0.1, 00:00:00, FastEthernet0/0
```


R1 ahora tiene tanto el [gateway of last resort](pseudo-trash/gateway%20of%20last%20resort.md) como el next-hop address de R2.

```
R1#show ip route
**Gateway of last resort is 10.0.0.2 to network 0.0.0.0**
C    172.16.0.0/16 is directly connected, FastEthernet0/0
10.0.0.0/30 is subnetted, 1 subnets
C       10.0.0.0 is directly connected, Serial0/0
**R*   0.0.0.0/0 [120/1] via 10.0.0.2, 00:00:15, Serial0/0**
```

Cada protocolo tiene diferentes opciones disponibles con el comando `default-information originate`, [[OSPF]]  ofrece `always`, cuando es establecido, este le dice al router que anuncie una ruta por defecto a otros routers incluso si no tienen una ruta por defecto en la tabla de enrutamiento. 