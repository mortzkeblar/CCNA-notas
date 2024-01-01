Es infraestructura de red bajo una administración comun.

![](_anexos_/Screenshot%20from%202023-12-27%2016-27-41.png)

- Estos sistemas corren dentro de sus redes algun tipo de `IGP`. 
- Como cada IGP es autónomo y esta dentro de una organización no hay problema respecto al uso. La cuestión esta al querer conectar esas redes con IGPs diferentes.
	- La conexión que permite eso es `EGP`, hoy en dia el único protocolo de ese tipo es  `(BGP)` .

![](_anexos_/Screenshot%20from%202023-12-27%2016-31-42.png)

> Como los BGP son direcciones publicas. Se puede buscar información en internet. Key: `BGP looking glass`.

Las IGP estan en potestad de elegir el rango de direcciones privadas para su uso, ya que esas direcciones no van a ser enrutadas a internet. Por eso hay ISPs que asignan IPs privadas del tipo `10.0.0.x` o `192.168.0.0` sin generar conflictos. 
