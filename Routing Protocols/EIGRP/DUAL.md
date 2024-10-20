Diffusing Update Algorithm (DUAL) es el algoritmo que usa [[EIGRP]] para determinar el mejor camino (el caminio loop-free con la menor [[metric]]) a una red de destino. 
- La ruta seleccionada es conocida como _Successor route_
- La metrica seleccionada es conocida como _Feasible distance (FD)_
- El next-hop router por el que pasa el _successor route_ (el router que anuncio la ruta) es llamado _successor_

El _successor_ (next-hop router), anuncia la ruta con su propia [[metric]] (tambien conocido como _reported distance (RD)_ o distancia anunciada). 

La _feasible distance_ incluye la distancia informada y el [[cost]] para llegar al _successor_. 

> La _successor route_ se incluye en la IP [[routing table]] y en la EIGRP topology table, con el next-hop neighbor como _successor_.

EIGRP usa la _condición de viabilidad_ para la prevención de loops, la cual consiste en que las rutas anunciadas (RD) por los candidatos factibles a _successor_ sean menores a la FD.

#### local computation
Cuando ocurren cambios que en la topologia que hace que el _successor_ es eliminado o cambiado, DUAL revisa por los candidatos factibles a _successor_ para esa ruta, cuando encuentra uno, DUAL lo utiliza para no volver a calcular nuevamente la ruta. Esto es llamado calculo local y es posible gracias a que el _factible successor_ ya ha sido elegido antes de que el _successor_ o la ruta primaria fallé. 

En caso de que no exista un _feasible successor_, el router envia una query a sus vecinos preguntando si tienen información sobre la red de destino. En caso de que sí, el router comienza a hacer el diffusing computation para determinar el nuevo _successor_. 

![[Pasted image 20240905061929.png|500]]

## EIGRP tables 
[[EIGRP]] usa DUAL para generar tres tablas: neighbor, topology and routing. 
### neighbor table 
Esta tabla mantiene la lista de router donde se haya formado adyacencias. Se puede consultar con `show ip eigrp neighbors`

![[Pasted image 20240905062115.png|500]]
``` 
R2#show ip eigrp neighbors
IP-EIGRP neighbors for process 1
Address Interface Hold Uptime SRTT RTO Q Seq Num
(ms) (ms) Cnt
192.168.1.1 Fa0/0 11 00:21:33 360 2160 0 7
```
### topology table 
La tabla de topologia es donde se encuentran todas la rutas aprendidas que pueden ser encontradas. Todas las _feasibles distances_ y [[metric]]s se almacenan aqui. La cual puede ser consultada con `show ip eigrp topology`

``` 
R2# show ip eigrp topology
IP-EIGRP Topology Table for AS(1)/ID(192.168.1.2)
Codes: P - Passive, A - Active, U - Update, Q - Query, R - Reply,
r - reply Status, s - sia Status
P 192.168.1.0/24, 1 successors, FD is 281600
via Connected, FastEthernet0/0
P 172.16.0.0/16, 1 successors, FD is 409600
via 192.168.1.1 (409600/128256), FastEthernet0/0
```

- Passive - no se realizan calculos EIGRP para llegar a este destino 
- Active - se estan realizando calculos EIGRP 
- Update - un Update packet fue enviado para este destino 
- Query - un Query packet fue enviado 
- Reply - un Reply packet fue enviado 
- SIA - hace referencia a _stuck in active_, esto significa que EIGRP no ha recibido respuesta a la consulta de un vecino en el tiempo estimado (3 minutos aprox)

> Para que una ruta se instale en la topology table, esta debe cumplir la condición de viabilidad 
### routing table 
En esta tabla se alojan las rutas con la [[metric]] compuesta más baja, se puede consultar con `show ip route`.
> Si una ruta falla, EIGRP puede reemplazar rapidamente la ruta usando la topology table para encontrar al nuevo _successor_. 

``` 
R2#show ip route
Codes: C - connected, S - static, R - RIP, M - mobile, B – BGP,
D - EIGRP, EX - EIGRP external, O - OSPF,
IA - OSPF inter area
Gateway of last resort is not set
D 172.16.0.0/16 [90/409600] via 192.168.1.1, 00:22:07, F0/0
C 192.168.1.0/24 is directly connected, F0/0
```
