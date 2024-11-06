---
tags:
  - routing
  - static
  - CCNA
---
Esto es otro de los metodos por los cuales un dispositivo de capa 3 puede aprender rutas (junto con [[connected route]], [[local route]] y [[dynamic routing]]). Consiste en la agregación de rutas que no estan conectadas directamente al router, y se realiza de forma manual por el administrador. 

![[Pasted image 20241101010939.png|600]]
En caso de que no se configuren rutas manualmente los dispositivos no van a poder llegar a su destino si se trata de una red que no se encuentra [[connected route]]. 

![[Pasted image 20241101011056.png|500]]

El criterio para añadir las rutas que un router no conoce para que existe una comunicación bidireccional entre dos dispositivos es la de que _cada router solo necesita saber las rutas hacia las redes de los hosts (emisor y receptor)_. 

![[Pasted image 20241101011858.png|600]]

En el ejemplo, se puede ver que cada router necesita una ruta hacia la red `192.168.3.0/24` para realizare el forwarding de PC1 a PC3, luego se sigue la misma lógica pero para con la red `192.168.1.0/24` para el forwarding de PC3 a PC1. 

![[Pasted image 20241101012328.png|500]]
Este gráfico indica las rutas estaticas que deben configurarse, sobreentiende las [[connected route]] que ya tiene cada router conectado al host correspondiente. 

## Configuring static routes 

Para configurar rutas estáticas se usa el comando `ip route` el cual tiene una serie de variantes que se pueden usar. 

```
ip route <destination-network> <netmask> <next-hop>

ip route <destination-network> <netmask> <exit-interface>

ip route <destination-network> <netmask> <exit-interface> <next-hop>
```

### Static routes specifying the next-hop IP address

![[Pasted image 20241101013309.png]]
Una ruta estática en la que solo se especifica el next-hop IP address es llamada un _recursive static route_. El motivo es que la ruta requiere de multiples busquedas en la tabla de enrutamiento para el forwarding de un paquete. 
- Una busqueda para encontrar el next-hop IP address 
- Una busqueda para encontrar la interface a la que esta conectada el next-hop IP address 

En el siguientes ejemplo, cuando R1 recibe un paquete destinado a 192.168.3.11. Este encuentra que la ruta coincidente más especifica es la ruta estatica que se ve en la imagen. 

![[Pasted image 20241101014734.png|600]]

> Se destaca la importancia de saber el puerto por el cual debe salir el paquete, ya que tambien se debe tener la MAC address de next-hop para el ethernet header. 

### Static routes specifying the exit interface 
Tambien llamada _directly connected static route_, esto es porque en la [[routing table]] aparece como una ruta conectada directamente. 
``` bash
R1(config)# ip route 192.168.3.0 255.255.255.0 g0/0
```
![[Pasted image 20241105053810.png|500]]
El mayor problema de esta configuración es que el router al creer que tiene una ruta conectada directamente, este va a enviar los frames con destino al host final en vez de dirigirlas al dispositivo next-hop.

Esto implica por ejemplo que el router no va a enviar un [[ARP]] request para conocer la next-hop  MAC address sino que va a enviar el [[ARP]] request directamente al host de destino (algo que no es posible ya que el mensaje broadcast no va a salir de su red local). Otro problema es que el router tiene que crear una entrada ARP para cada host final al que quiera enviar paquetes, a diferencia de otros metodos en los cuales solo necesita a la mucho una MAC address (la del next-hop).

![[Pasted image 20241105055919.png|500]]

Si bien esto se puede solventar si el dispositivo receptor tiene habilitado [[ARP]] Proxy (enabled en Cisco por defecto), el cual toma la solicitud del router y la reenvia al host de destino. Añade complejidad adicional, por lo cual se recomienda no usar este tipo de ruta estática. 

### Static routes specifying the exit interface and next-hop IP address
Tambien llamada _fully specified static route_, el beneficio de esta configuración es que el router sabe tanto la IP address next-hop como la interface por donde salir por lo cual no necesita realizar una busqueda recursiva.  

``` bash
R1(config)# ip route 192.168.3.0 255.255.255.0 g0/0 192.168.12.2
```
![[Pasted image 20241105050032.png|500]]

## connected
- [static route configuration](static%20route%20configuration.md) 
- [(OLD) static route configuration (IPv6)]((OLD)%20static%20route%20configuration%20(IPv6).md) 
- [gateway of last resort](pseudo-trash/gateway%20of%20last%20resort.md) 
- [IP default-gateway (cisco command)](IP%20default-gateway%20(cisco%20command).md) 
- [IP route 0.0.0.0](pseudo-trash/IP%20route%200.0.0.0.md) 
- [IP default-network](pseudo-trash/IP%20default-network.md) 
- [static host routes](pseudo-trash/static%20host%20routes.md) 
- [permanent routes](permanent%20routes.md) 
- [floating static route](floating%20static%20route.md) 


