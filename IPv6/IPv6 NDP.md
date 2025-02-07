---
tags:
  - CCNA
---
En [[IPv6]] a diferencia de [[IPv4 addressing|IPv4]] (tal como se describe en [[How end hosts send packets]]), no usa [[ARP]] para el reenvio de paquetes al next-hop.

IPv6 usa el procotolo **Neighbor Discovery Procotol (NDP)** que tiene un rol similar a [[ARP]], realiza el mapping de direcciones [[Layer 3]] IPv6 a [[Layer 2]] MAC address. 

### Solicited-node multicast
Algunas funciones de NDP usan una dirección llamada _solicited-node multicast address_, este se genera a partir de una dirección de host unicast ([[IPv6 address types#Global unicast]], [[IPv6 address types#Unique local]] o [[IPv6 address types#Link-local]]).

Para generar un solicited-node multicast address, anteponer `ff02:0000:0000:0000:0000:0001:ff` (abrev. `ff02::1:ff`) a los ultimos 6 digitos hexadecimales de la _unicast address_.

![[Pasted image 20241211223725.png]]

> El alcance de un solicited-node multicast address es [[IPv6 address types#Link-local]]

Algunos ejemplos de solicited-node multicast address a partir de unicast address:

![[Pasted image 20241211223920.png]]

Al ejecutar `show ipv6 interface <interface>` podemos ver la dirección solicited-node multicast generada para nuestra dirección unicast. 

![[Pasted image 20241211224049.png]]

### IPv6 address resolution with NDP 
Consiste en el mapping de direcciones IPv6 de [[Layer 3]] a direcciones MAC de [[Layer 2]], NDP usa dos tipos de mensajes ICMPv6.
- _Neighbor Solicitation (NS)_ - ICMPv6 type 135 
- _Neighbor Advertisement (NA)_ - ICMPv6 type 136

> NDP se considera un componente de ICMP, es protocolo cumple varias funciones tanto en IPv4 como en IPv6.

![[Pasted image 20241211224656.png]]

> Un mensaje [[ARP]] request es de tipo broadcast, pero en IPv6 no existe el concepto de broadcast. Aca es donde se aplica el concepto de _solicited-node multicast address_.
> 

- Para aprender una MAC address de un vecino, el host envia un mensaje _Neighbor Solicitation (NS)_ (equivalente a un ARP request) a la dirección _solicited-node multicast_
- El destino del mensaje NS respondera con un _Neighbor Advertisement (NA)_ (equivalente a un [[ARP]] reply) informando la MAC address
	- El mensaje _NA_ es de tipo unicast 

La tabla en la que se mapean las relaciones de direcciones [[Layer 2]]  - [[Layer 3]]  se llama _[[IPv6]] neighbor table_, esta puede ser consultada con `show ipv6 neighbor`.

![[Pasted image 20241211231024.png]]

### Router discovery with NDP
NDP tambien proporciona _router discovery_, esto permite a los hosts detectar automaticamente otros dispositivos conectados dentro de la red local. En este caso se usan dos tipos de mensajes para el descubrimiento de hosts:
- _Router Solicitation (RS)_ - ICMPv6 type 133
- _Router Advertisement (RA)_ - ICMPv6 type 134

Los mensajes _RS_ se usa para solicitar a todos los routers en la red local que se identifiquen, estos mensajes se envian a la _link-local multicast address_ `ff02::2`.

Los mensajes _RA_ se envian en respuesta a un _RS_, se envian de forma periodica en la red (incluso si no reciben un RS). Los mensajes RA son enviados normalmente a la _link-local multicast address_ `ff02::1`.

#### SLAAC 
_Router discovery_ se usa para facilitar el **Stateless Address Autoconfiguration (SLAAC)**, SLAAC permite a un host generar automaticamente su propia [[IPv6]] address, además de obtener información como el [[Default gateway]]. 

> SLAAC es _stateless_ porque no hay un server central que mantiene el tracking sobre las direcciones que obtienen los hosts. Contrario a IPv4 en el cual se emplea un servidor DHCP.

Para generar un [[IPv6]] usando SLAAC, en cisco se usa `ipv6 address autoconfig`.
- Para la porción de host, SLAAC usa [[IPv6#Modified EUI-64]]
- Se puede usar `ipv6 address autoconfig default` que realiza lo mismo pero tambien agrega una _default route_ en la [[Routing Table]] usando el link-local address del router que responde como next-hop. 

![[Pasted image 20241211233300.png]]

### Duplicate address detection 
NDP _Duplicate Address Detection (DAD)_ es una caracteristica que verifica si una IPv6 address es unica en la red antes de ser usada. Esta se ejecuta: 
- Cuando se configura una IPv6 ya sea manual o automatica. 
- Cuando se habilita una interface (UP / UP state)

> Una IPv6 address es provisional y no puede ser usada para la comunicación hasta que haya pasado la verificación _DAD_

DAD tiene dos tipos de mensajes:
- _Neighbor Solicitation (NS)_ - un host envia un mensaje a su propio _solicited-node multicast_ address, si no recibe una respuesta en un periodo de tiempo se puede asegurar que la dirección es unica y segura de usar 
- _Neighbor Advertisement (NA)_ - si un host recibe este mensaje al enviar un _NS_ significa que ya hay otro host en la red con ese IP, entonces es marcada como duplicada y no se le permitira usar esa dirección para la comunicación.


![[Pasted image 20241211235145.png]]

![[Pasted image 20241211235451.png]]

