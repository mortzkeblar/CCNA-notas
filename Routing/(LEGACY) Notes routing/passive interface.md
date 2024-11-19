---
tags:
  - routing
  - dynamic
  - CCNA
---

Esto se utiliza para que un router no participe de forma activa en protocolo de enrutamiento, en caso de los [Dynamic Routing](Dynamic%20Routing.md) protocols (como EIGRP, [RIPv2](../RIP/RIPv2.md) o [OSPF](../OSPF/OSPF.md)), estos envian/reciben mensajes para poder conectarse entre sí, a través de sus interfaces. 
Si desea que un interface sea pasiva:
``` bash
passive-interface <INTERFACE>
```

Esto implica (en [RIPv2](../RIP/RIPv2.md)) que la interface del router no puede enviar actualizaciones de enrutamiento, aunque si puede recibirlos.

En EIGRP y OSPF, una interface pasiva supreme los Hellos, por lo que no se forman relaciones entre vecinos, esto significa que esos routers no puede recibir ni anunciar ninguna actualización de enrutamiento.

``` bash
R1#conf t
Enter configuration commands, one per line.  End with CNTL/Z.
R1(config)#router eigrp 10
R1(config-router)#network 192.168.1.0
R1(config-router)#passive-interface fast0/0
```

Si desea que actualizaciones de enrutamiento se envia a un host determinado conectado a `fast0/0` pero no quiere actualizaciones broadcast (o multicast a `244.0.0.9`) puede usar el comando `neighbor` para enviar mensajes unicast hacia el router vecino.

``` bash
R1(config-router)#neighbor 192.168.1.10
```

`neighbor` puede ser usado en redes non-broadcast, como los frame relay, para permitir que actualizaciones unicast pasen a través del medio.


### linked 
[passive interface in RIP](RIP/passive%20interface%20in%20RIP.md) 
