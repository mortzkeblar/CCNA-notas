---
tags:
  - routing
  - OSPF
  - dynamic
---
_Ver: [OSPF overview](OSPF%20overview.md) _

[OSPF](OSPF.md) esta diseñado para localizar rapidamente los cambios locales dentro de una topologia de red y asi calcular el [best routing path selection criteria](best%20routing%20path%20selection%20criteria.md). La red completa se divide en areas, dentro de estas areas se inunda la red de LSAs (link state advertisements) para los routers vecinos. Esto asegura que los routers en otras areas [OSPF](OSPF.md) no son afectados por los cambios o problemas como p. ej. caida de enlaces (links flapping). 

Contrario a [RIP](RIP.md), [OSPF](OSPF.md) funciona con una jerarquia de areas. En el nivel más alto esta los [autonomous system](autonomous%20system.md) (AS), una construcción logica para una colección de redes bajo un administrador comun. 
- OSPF es un protocolo _Intra-AS (interior gateway)_ 
- Un protocolo externo se encarga de intercambiar información entre los distintos [autonomous system](autonomous%20system.md). 

Un AS puede dividirse en varias areas con direccionamiento de red continua, dependiendo los requisitos en ese momento. Los routers con multiples interfacces pueden participar en multiples areas si se requiere. Estos routers son llamados _area border routers_ y mantienen bases de datos topológicas separadas para cada area.

Las areas sirven como limites para la inundación de la red con anuncios LSAs que genera [OSPF](OSPF.md) (algunos LSAs solo inundan dentro de un area). Los routers de una misma area contienen la misma DB routing. Un router con todas sus interfaces dentro de un mismo area es conocido como _internal router_. 

[OSPF](OSPF.md) solo funcionara si el router tiene al menos un interface activa, este enviara paquetes Hello usando `224.0.0.5` (tambien llamdo ALLSPFRouter address). Si el enlace OSPF esta en un NBMA (como Frame Relay), el paquete OSPF sera unicast en lugar de multicast. 

Un vez el paquete OSPF es verificado por el router receptor, se forma una relación de vecinos (neighbor) entre los dos routers. Cada uno envia su _Link State Database_ a los demas routers OSPF, de esta forma se crea un camino loop-free para cada ruta. 

Una _Topological Database_ contiene la colección de LSAs recibidos desde todos los routers que estan SOLO en la misma area. Dado que estos routers comparten la misma información dentro del mismo area, tiene DB topologicas identicas. 

Cuando se crean multiples areas, se pueden ver dos tipos de rutas OSPF dependiendo si el origen y el destino estan en el mismo o en diferentes area. Cuando se produce el segundo caso se le denomina enrutamiento _inter-area_. 

Un [OSPF](OSPF.md) backbone (conocido como Area 0) es responsable de distribuir la información de enrutamiento entre otras areas non-zero. Todos los routers con al menos una interface en el area 0 se les conoce como _backbone routers_. 


- Cualquier router con todas sus interfaces dentro del mismo area es denominado _internal router (IR)_
- Cualquier router que actúe como conexión entre routers con distintos protocolos o instancias de [OSPF](OSPF.md) se lo conoce como _autonomoues system boundary router (ASBR)._
- Cualquier router con interfaces en más de un area (area 0 y otra area) es conocido como _area border router (ABR)_