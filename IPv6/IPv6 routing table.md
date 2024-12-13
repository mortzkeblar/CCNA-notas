La tabla de enrutamiento en [[IPv6]] cumple las mismas funciones que la [[Routing Table]] en IPv4, con el agregado de que tambien aprende las _connected_ y _local_ routes cuando se configura una IP address en una interface. 

## Connected and local routes 
Una _[[Connected route]]_ es una ruta a una red que esta conectada a una interface. 

Una _[[Local route]]_ es una ruta a la IP address exacta configurada en la interface del router. 

![[Pasted image 20241212000639.png]]

> Nota: no olvidar ejecutar `ipv6 unicast-routing` al usar [[IPv6]]

Para ver las IPs configuradas en las interfaces usamos `show ipv6 interface brief`. Vemos que además de las IPs configuradas tambien se agregaron direcciones _link-local_ usando [[IPv6#Modified EUI-64]]

![[Pasted image 20241212001213.png]]

Con `show ipv6 route` vemos la salida de la IPv6 routing table. 

![[Pasted image 20241212001521.png]]
- R1 agrega una connected route y una local route por cada dirección configurada en las interfaces 
- Una local route es un ejemplo de _host route_, una ruta a una unica IP address. 
	- Los IPv6 host routes tiene el `/128` prefix length
- Una connected route es un ejemplo de _network route_, una ruta a más de un IP address de destino. 
	- Cualquier IPv6 route con `/127` prefix length o menor es una network route 

Se puede observar de que no hay _local_ ni _connected_ routes para los _link-local addresses_, esto es así ya que estas direcciones no necesitan ser enrutadas de forma tradicional ya que nunca salen del _local link_.
- Una link-local address puede ser usada como next-hop address de una ruta, pero nunca como el destino de una ruta. 

La ruta final en la routing table de R1 es una ruta a `ff00::/8`, el _multicast address range_. Esta ruta se inserta forma automatica por el router y esta asociada con la interface virtual `Null0`, los paquetes enviados por estas ruta son dropped.
- Los routers Cisco no reenvian los paquetes multicast por defecto, por lo que esta ruta asegura que todos esos paquetes sean droppeados. 
	- Esto no previene al router de enviar o recibir paquetes multicast, ya que esos paquetes son usados por ejemplo en procesos [[NDP]]. Solo se limita al reenvio de los paquetes multicast.


## IPv6 route selection 
En IPv6 este concepto es igual al explicado en [[Routing Table#Route selection]].

![[Pasted image 20241212010653.png]]

Cuando un paquete ingresa a R1, este debe tomar la decisión sobre que hacer con el paquete. Dos de las rutas hacen match con la IPv6 address de destino del paquetes, de estas dos rutas, R1 elige la ruta más especifica que es la más _longer prefix match_ (`/128`). 

En caso de que el paquete no haga match con ninguna de las rutas, este es dropped.

