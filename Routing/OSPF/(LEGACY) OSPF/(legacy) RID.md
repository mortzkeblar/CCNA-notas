---
tags:
  - routing
  - dynamic
  - OSPF
  - CCNA
aliases:
  - Router ID
date created: Saturday, October 19th 2024, 11:08:14 pm
date modified: Wednesday, November 20th 2024, 11:33:50 pm
---

Cada router tiene su propio ID unico de 32-bit basado en la interface, esto sirve para [OSPF](../OSPF.md) como identificador de cada router, el cual ayuda a detectar facilmente LSAs y endpoints duplicados de enlaces virtuales, así como para determinar los desempates entre DR y DBR. 

El router ID (RID)  se toma en base a:
- Si existe una dirección Loopback, será elegida por encima de cualquier otra dirección IP.
	- Si existen varias direcciones Loopback, se elige la que tiene la [IP address](IP%20address.md) más alta.
- Si no existe una dirección Loopback, se elige a la [IP address](IP%20address.md) más alta del router.

La interface del que sale el RID no tiene que estar bajo [OSPF](../OSPF.md) o participando en el proceso [OSPF](../OSPF.md). Los administradores de red suelen asignar una dirección Loopback porque así pueden asignar un RID predecible, ademas que al ser una interface virtual, nunca puede estar _down_. 
Esto ultimo también es condición necesaria para que [OSPF](../OSPF.md) puede actuar sobre la interface, sobretodo al bootear el router, debe asegurarse que la interface esta _up_. En caso de que no, puede reiniciar el proceso [OSPF](../OSPF.md) con `clear ip ospf process`. 

``` bash
Router#show ip interface brief
Interface IP-Address OK? Method Status Protocol
Ethernet0 unassigned YES unset administratively down down
Loopback0 172.16.1.1 YES manual up up
Serial0 192.168.1.1 YES manual up up
Serial1 unassigned YES unset administratively down down

Router#show ip ospf 20
Routing Process ospf 20 with ID 172.16.1.1 OSPF router ID
```

Para configurar un RID con una dirección Loopback alta (p. ej. 192.168.100.100) o bien usando `router-id` lo cual es una buena practica.

``` bash
Router#config t
Router(config)#router ospf 20
Router(config-router)#router-id 192.168.100.100
```


