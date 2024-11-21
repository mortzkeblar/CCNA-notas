---
tags:
  - CCNA
  - SpanningTreeProtocol
date created: Sunday, November 17th 2024, 2:17:47 am
date modified: Wednesday, November 20th 2024, 11:33:50 pm
---
Como su nombre indica, _Loop Guard_ da protección contra los layer 2 loops en la [[LAN]]. Añadiendo una una capa de protección adicional contra la transición erronea de un puerto desde un _discarding state_ (_blocking state_ en [[Project/Networking/CCNA-notas/Spanning Tree Protocol/STP|STP]]) hacia un _forwarding state_. 

Esto puede suceder cuando un discarding port deja de recibir BPDUs, haciendo que el [[switch]] crea que puede pasar el puerto al forwarding state sin causar un loop. 

![[Pasted image 20241116234636.png]]
En este caso se produce un loop cuando SW3 deja de recibir BPDUs desde SW2 (por algún hipotetico problema), una vez que se termina el _max age timer_. SW3 convierte el puerto en designated y pasa al estado forwarding. Los tres enlaces entre los [[switch]]es estan activos, lo que desencadena un layer 2 loop. 

Para evitar esto se puede habilitar Loop Guard en el _alternate port_ con `spanning-tree guard loop`. 
- Cuando Loop Guard esta habilitado, si el puerto deja de recibir BPDUs esta pasa al _loop-inconsistent_ state, deshabilita el puerto lo que hace que no pase por el forwarding state. 
- Si SW2 (en la imagen) recupera el puerto y comienza a enviar BPDUs nuevamente, el puerto de SW3 se recupera automaticamente y vuelve a su estado normal (discarding)

![[Pasted image 20241117002649.png]]e
> Importante: Root Guard y Loop Guard son mutuamente excluyentes, no se puede habilitar ambas caracteristicas en un puerto de forma simultanea. Esto debido a que tiene cumplen diferentes roles y objetivos. 
> - Root Guard toma acción basado en si recibe BPDUs de valor superior 
> - Loop Guard toma acción en caso de que no reciba BPDUs 
