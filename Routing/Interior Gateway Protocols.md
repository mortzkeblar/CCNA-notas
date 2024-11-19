---
tags:
  - CCNA
---
Los _Interior Gateway Protocols (IGPs)_ se pueden dividir según sus _algorithm types_: distance-vector y link-state.

## Distance-Vector Protocols 
Los principales protocolos basados en _distance-vector_ son: 
- Routing Information Protocol ([[RIP]]) - un protocolo simple que es usado en pequeños entornos y labs de prueba
- _Cisco_ Enhanced Interior Gateway Routing Protocol ([[EIGRP]]) - tambien denominado como un _advanced distance-vector protocol_. Se usado en redes de gran escala que tienen a Cisco como su principal proveedor. 
	- EIGRP fue desarrollado como un protocolo propietario, que luego fue lanzado al publico en el _RFC 7868_. Aun asi, no hay muchas implementaciones de [[EIGRP]] más alla de las que vienen en los equipos de Cisco. 

Los routers que usan un _distance-vector_ protocol, comparten la information sobre sus redes conocidas y sus **_metrics_** para llegar a esas redes. 
- Las _metrics_ son un concepto similar al _[[Project/Networking/CCNA-notas/Spanning Tree Protocol/STP|STP]] Root Cost_, esta mide la eficiencia de una ruta para llegar al root bridge. Igualmente las _metrics_ miden la eficiencia de una ruta para llegar a la red de destino. 

![[Pasted image 20241118152209.png]]
En la imagen se puede ver como los routers aprenden las rutas para llegar a la red LAN A (en este caso a través de [[RIP]]).

> La característica clave en los _distance-vector_ protocols es que los routers _no tienen un mapa completo de la red_, para cada destino que aprende. Solo tiene la _metric_ y el _next-hop_ router.

![[Pasted image 20241118155300.png]]

Los enrutamiento de tipo distance-vector tambien es llamado por este motivo _routing by rumor_. Ya que no tiene un mapa completo de la red, solo tiene lo que les dicen sus vecinos. 

## Link-State Protocols 
Los principales protocolos basados en _link-state routing_ son:
- Intermediate System to Intermediate System (IS-IS) - comúnmente usado en _service-providers network_
- Open Shortest Path First ([[OSPF]]) - el protocolo de enrutamiento IGP más usado y el tema principal de la certificación CCNA. 

Cuando se usa un protocolo link-state, cada router tiene un _connectivity map_ de la red. Cada router comparte información sobre sus enlaces conectados y los estados que tienen (subredes conectadas, metric cost, etc). Esta información es compartida a todos los routers de la red, de la que luego se forma el _connectivity map_ de toda la red por la cual todos los routers obtienen el mismo mapa.

Los routers usan este mapa para calcular la mejor ruta a un determinado destino en la red.
![[Pasted image 20241118174521.png]]

Una desventaja frente a los distance-vector protocols, es que tanto la contrucción del _connectivity map_ como el calculo de rutas son procesos que son intensivos en el uso de CPU y memoria. El mapa de la red se almacena usando una estructura llamada **_Link-State Database (LSDB)_**, calcular las rutas a partir del LSDB puede ser CPU-intensivo. Para solvertar estos problemas se recurre a la división de redes dentro de _areas_ ([[OSPF area]] i.e).

