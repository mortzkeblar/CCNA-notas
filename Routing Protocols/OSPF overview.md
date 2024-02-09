[OSPF](OSPF.md) es un protocolo [classless](../classless.md), descubre y mantiene una relación con los routers vecinos mediante el envio de paquetes multicast _Hello_ y de timers _Dead_. Las actualizaciones toman la forma de _Link State Advertisements (LSAs)_. El link state database es _OSPF's topology table._ Los LSAs inundan las areas [OSPF](OSPF.md) hasta que cada router tiene un mapa consistente de la red. (p. ej. todos los link state database hacen matching). 

Una vez que el mapa es consistente, SPF corre sobre la base de datos y se construye un ruta loop-free (sin bucles) para cada red de destino. Esto se denomina _SPF tree,_ puede ser vista desde la tabla de enrutamiento. 

[OSPF](OSPF.md) usa una [metrics](metrics.md) arbitraria de costo para determinar el camino más corto de A a B.

- Los routers configurados con OSPF envia paquetes Hello fuera de todas las interfaces OSPF. Si el router en el enlace compartido esta de acuerdo con los paquetes Hello enviados, se convierten en vecinos.
- Algunos vecinos forma adyacencias. Esto depende de el tipo de router [OSPF](OSPF.md)  y la red, como un point-to-point o broadcast.
- _Link State Advertisements_ son enviadas a través de cada adyacente, con información sobre los enlaces de los routers, vecinos y el estado del enlace.
- Cada router construye un mapa idéntico o una base de datos _link state_. 
- Una vez que todos los routers comparten la misma database, este corre el algoritmos SPF para calcular el mejor camino loop-free para cada destino de la red. Por defecto cada router se considera a si mismo el Root SPF.  
- Cada router puede ahora construir su tabla de enrutamiento.