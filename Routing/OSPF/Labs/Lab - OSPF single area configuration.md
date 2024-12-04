---
tags:
  - routing
  - dynamic
  - OSPF
  - CCNA
date created: Saturday, October 19th 2024, 11:08:14 pm
date modified: Wednesday, November 20th 2024, 11:33:50 pm
---

El [OSPF](../OSPF.md) process ID es localmente significativo y no necesita ser el mismo en todos los routers dentro un área o la red entera. Puedes incluso tener más de un [OSPF](../OSPF.md) process corriendo en el mismo router. En las redes del mundo real, muchas veces el OSPF process ID se mantiene unico para hacer el troubleshooting más facil. 

![](15-6-scaled.jpg)


OSPF puede ser configurado en dos pasos para redes simples (normalmente con un solo area).

Definir OSPF en el router:

``` bash
Router(config)#router ospf [process-id]
```

El process ID es un numero interno usado para identificar multiples instancias de OSPF corriendo en un router. Esto solo es significativo localmente, por lo que no necesita coincidir con otros routers, a la vez puede ser reutilizado en otros dispositivos. 

Asignar redes a el area OSPF relevante:

``` bash
Router(config-router)#network address wildcard-mask area [area ID]
```

- Address - la dirección de red 
- [(legacy)wildcard mask]((legacy)wildcard%20mask.md) 
- Area/area ID - es el area [OSPF](../OSPF.md) en que quieres que este la red/interface. Si estas usando más de una area, una de ellas debe ser el area 0. 

Para la red de la imagen, pon las dos interfaces fastethernet dentro del Area 0, tambien la interface Loopback en R2:

``` bash
# R1

R1(config)#int f0/0
R1(config-if)#ip add 192.168.1.1 255.255.255.0
R1(config-if)#no shut
R1(config)#router ospf ?
[1-65535] Process ID ï Note that the process ID can’t be 0
R1(config)#router ospf 1
R1(config-router)#network 192.168.1.0 0.0.0.255 area ?
[0-4294967295] OSPF area ID as a decimal value
A.B.C.D OSPF area ID in IP address format
R1(config-router)#network 192.168.1.0 0.0.0.255 area 0

# R2 (incluyendo interface Loopback)
R2#conf t
Enter configuration commands, one per line. End with CNTL/Z.
R2(config)#int f0/0
Technet24.ir
R2(config-if)#ip add 192.168.1.2 255.255.255.0
R2(config-if)#no shut
R2(config-if)#router ospf 1
R2(config-router)#net 192.168.1.0 0.0.0.255 area 0
R2(config-router)#end
R2#
*Mar 1 00:04:28.207: %OSPF-5-ADJCHG: Process 1, Nbr 192.168.1.1 on
FastEthernet0/0 from LOADING to FULL, Loading Done

R2(config)#int lo0
R2(config-if)#ip add 172.16.1.1 255.255.0.0
R2(config-if)#router ospf 1
R2(config-router)#net 172.16.0.0 0.0.255.255 area 0

## verificamos las rutas en R1
R1#show ip route
Gateway of last resort is not set
172.16.0.0/32 is subnetted, 1 subnets
O 172.16.1.1 [110/11] via 192.168.1.2, 00:01:21, FastEthernet0/0 #OSPF
C 192.168.1.0/24 is directly connected, FastEthernet0/0
```


## conclusions
Es importante notar que le comando `network` no se usa para anunciar subnets en OSPF, se usa para determinar que interfaces participan en [OSPF](../OSPF.md) y las subnet mask en esas interfaces en particular son las que determinan que subredes se anuncian. 
La [(legacy)wildcard mask]((legacy)wildcard%20mask.md)  no tiene que ser la inversa de la subnet mask configurada en una interface particular. Abajo esta una lista de comandos `show` necesarios para seguir la configuración.

``` bash
show ip route 

show ip ospf neighbor 

show ip ospf interface brief 

show ip ospf interface <INTERFACE>
```

Para añadir todas las interfaces del router al Area 0:

``` bash
Router(config)#router ospf 1
Router(config-router)#network 0.0.0.0 255.255.255.255 area 0
Router(config-router)#end
Router#show ip prot
Routing Protocol is “ospf 1”
[output truncated]
Maximum path: 4
Routing for Networks:
0.0.0.0 255.255.255.255 area 0
```

Cualquier [IP address](IP%20address.md) en cualquier interface habilitada sera añadida al Area 0 de [OSPF](../OSPF.md). 
