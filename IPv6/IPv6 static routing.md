---
tags:
  - CCNA
---
Al igual que otros conceptos de [[IPv6]], el enrutamiento estatico tiene los mismos principios que IPv4 [[Static Routing]]. 

![[Pasted image 20241212205600.png]]

Para que PC1 y PC3 puede comunicarse, cada router necesita tener rutas para llegar a sus respectivas redes (`2001:db8:1::/64` y `2001:db8:3::/64`).

Tanto R1 como R3 ya tienen [[Connected route]]s a sus las redes de PC1 y PC3. Entonces se deben configurar las siguientes rutas estaticas.

![[Pasted image 20241212210828.png]]

## Configuring IPv6 static routes 
Tenemos tres metodos para configurar rutas estaticas 
- Recursive static route - `ipv6 route <destination-prefix> <next-hop>`
- Directly connected static route - `ipv6 route <destination-prefix> <exit-interface>`
- Fully specified static route - `ipv6 route <destination-prefix> <exit-interface> <next-hop>`
### Recursive static routes
Es llamada así porque se necesita hacer busquedas recursivas en la table e enrutamiento. 
- Busqueda para encontrar la [[IP address]] del next-hop 
- Busqueda para la interface a la que esta conectada el next-hop 

Este concepto ya fue abarcado en IPv4 con [[Static Routing#Static routes specifying the next-hop IP address]].

### Directly connected static routes 
Es llamada así ya que esta tipo de ruta hace creer al router que esta directamente conectado a el destino especificado.

Este conceto ya fue abarcado en IPv4 con [[Static Routing#Static routes specifying the exit interface]]

![[Pasted image 20241212223305.png]]

> Es importante aclarar que este ruta es que NO funciona, por más que el router permita configurarla y que haya una entrada añadida en la tabla de enrutamiento. 

En IPv4 este mismo tipo de rutas acarrea problemas, que pueden ser parchados usando _proxy ARP_. Pero IPv6 no usa [[ARP]], usa [[IPv6 NDP]] y este protocolo no tiene un equivalente al _proxy ARP_.

Esto significa que [[IPv6 NDP]] no es capaz de aprender la MAC address de un host remoto, a menos que realmente se encuentre conectado a la interface. 

![[Pasted image 20241212223700.png]]

> Este tipo de ruta si funciona en _serial interfaces_ debido a su naturaleza point-to-point.

### Fully specified static routes 
Una _fully specified static route_ especifica tanto la interface de salida como el next-hop. Este concepto ya fue abarcado en IPv4 con [[Static Routing#Static routes specifying the exit interface and next-hop IP address]]

![[Pasted image 20241212225315.png]]

## Link-local next hops
Los paquetes que se originan o que tienen como destino una _link-local address_ no son enrutables, un router no realiza el forwarding de estos paquetes.

Sin embargo un _link-local address_ puede ser usado como next-hop de una ruta, este next-hop enruta el paquete al destino inmediato en el _link-local_. No enruta el paquete a través de multiples hops o segmetos de red.

![[Pasted image 20241212232359.png]]
En la imagen se puede ver la configuración de rutas estaticas usando los _link-local address_ como next-hops. 

Vemos que las _global unicast address_ entre los routers no aparecen representados en la imagen. Estas conexiones son ejemplos de enlaces _links - links_ que solo sirven para transportar entre las distintas partes de la red. 

Estos enlaces no son usados como destinos para los datos, por lo que no es necesario la comunicación remota en estos casos. Por esto el _link-local addressing_ es suficiente para nuestros propositos. 

![[Pasted image 20241213004532.png]]

Debido a los _link-local address_ tienen alcance limitado al enlace donde se configuran, estos enlaces nunca llegan a comunicarse entre sí. Lo que permite tener direcciones duplicadas sin generar conflicto.

Al usar una _link-local address_ como next-hop es importante que la ruta estática sea de la forma [[#Fully specified static routes]] para que no haya ambigüedad sobre por donde debería ir un paquete cuando se tiene más de una interface con la misma IP link-local address. 

![[Pasted image 20241213011537.png]]
De hecho, Cisco IOS no le permitira configurar una ruta con un link-local como next-hop si no es de la forma _fully specified_.

## Configuring a default route 
Una [[IPv6]] [[Default route]], es la ruta menos especifica posible. Una ruta a `::/0` que hace match con todo el rango de direcciones IPv6 posibles.

![[Pasted image 20241213012441.png]]

### Floating static routes 
Este concepto es igual que IPv4 [[Dynamic Routing#Floating static routes]]. Una ruta que pueda servir como backup para un ruta principal, esto se logra asignando una custom [[Dynamic Routing#Administrative Distance parameter]].

![[Pasted image 20241213012826.png]]

Tal como se define en [[Dynamic Routing#Route selection]], la ruta con valor más bajo de [[Dynamic Routing#Administrative Distance parameter]] es el que sera agregado a la [[Routing Table]]. 

En este caso la segunda ruta con un AD de `2` solo ingresara a la tabla de enrutamiento en caso de que la ruta principal haya sido removida. 

![[Pasted image 20241213013600.png]]