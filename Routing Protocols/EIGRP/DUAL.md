Diffusing Update Algorithm (DUAL) es el algoritmo que usa [[EIGRP]] para determinar el mejor camino (el caminio loop-free con la menor [[metric]]) a una red de destino. 
- La ruta seleccionada es conocida como _Successor route_
- La metrica seleccionada es conocida como _Feasible distance (FD)_
- El next-hop router por el que pasa el _successor route_ (el router que anuncio la ruta) es llamado _successor_

El _successor_ (next-hop router), anuncia la ruta con su propia [[metric]] (tambien conocido como _reported distance (RD)_ o distancia anunciada). 
La _feasible distance_ incluye la distancia informada y el [[cost]] para llegar al _successor_. 

> La _successor route_ se incluye en la IP [[routing table]] y en la EIGRP topology table, con el next-hop neighbor como _successor_.

