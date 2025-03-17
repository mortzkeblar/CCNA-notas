---
tags:
  - CCNA
---
**IPv6 protocol** es la siguiente versión del protocolo [[Project/Networking/CCNA-notas/IPv4 addressing/IPv4 addressing|IPv4]], esta surge para superar las limitaciones de IPv4. 

Como una dirección IPv4 se compone de 32 bits, este tiene un tamaño máximo de $2^{32}=4,294,967,296$ direcciones IP, lo que en el principio de la era de internet parecía un limite muy lejano, en realidad resulta que ya llegamos al limite de direcciones disponibles. LACNIC por ejemplo fue el ultimo RIR en anunciar el agotamiento de sus direcciones IPv4 en 2020. 

> La organización encargada de la asignación de direcciones IP en el mundo es la llamada **Internet Asigned Numbers Authority (IANA)**, la IANA a su vez distribuye los espacios IP en otros organizaciones llamadas **Regional Internet Registries (RIR)**.
> ![[Pasted image 20241208222613.png]]


IPv6 esta compuesto de 128 bits, lo que nos da la cantidad de $2^{128}=340,282,366,920,938,463,463,374,607,431,768,211,456$ direcciones disponibles. 

Más alla de las diferencias de tamaño y formato, estos cumplen la misma función que IPv4, IPv6 encapsula segmentos [[Layer 4]] (i.e TCP o UDP) con un header para formar paquetes. Este proporciona direccionamiento end-to-end desde el origen del mensaje al destino. Así mismo, el paquete IPv6 es encapsulado en un frame [[Ethernet Header (and Trailer)|Ethernet]] en cada hop hacia su destino final. 

### Hex representation 
IPv6 se representa usando el sistema [[hexadecimal]], esto significa que cada digito hex contiene 4 bits de información: $2^{4}=16$, tambien llamada _nibble_. 

![[Pasted image 20241208223932.png]]

## IPv6 header 
La cabecera IPv6 consta de los campos: version, traffic class, flow label, payload length, next header, hop limit, source address, destination address. Este ocupa un total de _41 bytes_.
![[Pasted image 20241208233857.png]]
- _Version field_ - al igual que [[Project/Networking/CCNA-notas/IPv4 addressing/IPv4 addressing|IPv4]], campo de 4 bits que esta configurado como $0b0110$ para indicar IPv6 
- _Traffic class field_ - 8 bits de tamaño, usado para [[QoS]] , este campo se divide en dos partes 
	- _Differentiated Service Code Point (DSCP)_ - 6 bits de tamaño 
	- _Explicit Congestion Notification (ECN)_ - 2 bits de tamaño 
- _Flow label field_ - 20 bits de tamaño, se usa para etiquetar flujos, un flujo es una sequencia de paquetes enviados desde un origen particular a un destino particular (i.e TCP session entre dos hosts)
- _Payload Length field_ - 16 bits de tamaño, se usa para indicar el tamaño del payload encapsulado del paquete 
	- A diferencia de [[IPv4 Header]], que tiene un valor variable (20 - 60 bytes), el IPv6 header tiene un tamaño fijo de 40 bytes. Esto permite que IPv6 solo necesite un campo que indica el tamaño del payload (a diferencia de IPv4 que necesita los campos _Internet header length_ y el _Total length_)
- _Next header field_ - equivalente al _Protocol field_ en [[IPv4 Header]], indica el tipo de mensaje encapsulado en el paquete. Algunos valores par este campo son:
	- 1: ICMP 
	- 6: TCP 
	- 17: UDP 
	- 58: ICMPv6 
	- 88: [[EIGRP]] 
	- 89: [[OSPF]] 
- _Hop limit field_ - equivalente al campo _Time to Lite ([[TTL]])_ de IPv4 
- _Source / Destination address_ - campos de 128 bits, que contiene las IPv6 de origen y destino 

## IPv6 address structure
En formato binario, una dirección IPv6 es un string de 128 bits ilegible para un humano. 
![[Pasted image 20241209000645.png]]

Por lo cual se realiza el agrupamiento en 8 grupos de 16 bits, separados por `:` y escritos en [[hexadecimal]]. 
- Un grupo de 8 bits también es llamado de forma informal _hextet_.
![[Pasted image 20241209001011.png]]
> Normalmente en IPv6 se usan prefix length `/64`, lo cual es extramadamente ineficiente ya que abarca un tamaño de $2^{64}$ direcciones. Pero es preferido debido a la simplicidad. 

### Abbreviating IPv6 addresses
Para simplificar más la representación de direcciones IPv6, existen dos metedos para el abreviamiento de direcciones. 
- _Eliminar los ceros a la izquierda_ - cualquier digito cero a la izquierda del hextet es eliminado.
	- Los ceros a la derecha no pueden ser eliminados,ya que en este caso el cero tiene un _peso_ en el valor del hextet (i.e. $0200 = 200$ pero $0200\ != 02$)
	- Si se tiene un hextet de $0000$ se simplifica un solo $0$
	
	![[Pasted image 20241209002137.png]]
-  _Omitir todos los hextet de ceros consecutivos_ - dos o más hextets en la que todos sus digitos estan en cero, se pueden omitir representandolos con un dos puntos dobles `::`
	- Debido a que IPv6 tiene 8 hextets de tamaño, se puede deducir la cantidad de hextet simplificados viendo el resto de hextets que se representan sin omitir
	- Este metodo solo puede ser usado una vez, independiente de que haya posibilidad de aplicar el metedo más de una vez. 
		- Se recomiendo aplicarlo en la zona donde haya mayor posibilidad de simplificación. Si ambas zonas son iguales, es preferible simplificar el que este más a la izquierda.  
	
	![[Pasted image 20241209002712.png]]

Ambos metodos pueden ser combinados para simplificar direcciones IPv6. 
![[Pasted image 20241209003408.png]]

**IPv6 address abbreviation table**
![[Pasted image 20241209004354.png]]

### IPv6 Prefix 
Un _network prefix_ es la combinación de una _network address_ y una _prefix length_, como se ve en [[IPv4 addressing]], este concepto es igual para IPv6. 

Un motivo por el cual los prefix length `/64` son tan usados es que no necesitas convertir a binario, solo los ultimos 4 hextets. Los porción de host se puede omitar si se convierte todos a ceros. 

**IPv6 network prefixes**
![[Pasted image 20241209005220.png]]

## IPv6 address configuration 
En [[Cisco IOS CLI]], los comandos para configurar IPv6 pasan son identicos a IPv4, solo se reemplaza `ip` por `ipv6`.
- `ip address` - `ipv6 address`
- `ip route` - `ipv6 route`
- `show ip interfaces brief` - `show ipv6 interfaces brief`
- `show ip route` - `show ipv6 route` 

![[Pasted image 20241209005524.png]]
Es importante ejecutar `ipv6 unicast-routing` en el modo global config al usar IPv6 ya que sino no se va poder enrutar los paquetes IPv6. 

### Manually assigning an IPv6 address 
La forma de configurar una dirección IPv6 es como `ipv6 address <address>/<prefix-length>`
- En este caso se usa la _slash notation_
- Se puede configurar direcciones en su forma abreviada
	- Cuando se quiere ver una IPv6 en un router Cisco, esta la muestra en la forma abreviada más corta posible (independientemente de la forma como se configuro).
- En IPv4, al configurar una IP en una interface se sobreescribe (si es que hay) la anterior IP configurada. En IPv6, al configurar más de una IP esta no se sobreescribe sino que se añade a la interface, quiere decir que se puede tener varias IPv6 configuradas en un interface. 

### Modified EUI-64 
**Modified Extended Unique Identifier 64 (EUI-64)** es un metodo para generar automaticamente un identificador de una interface IPv6 de 64 bits, que se usa como porción de host de la dirección. Para configurar el EUI-64 se debe agregar el string `eui-64` al configurar una IPv6 address. 

![[Pasted image 20241209020244.png]]
Nota: en `G0/2` se puede ver que tanto la IPv6 configurada como la dirección generada automaticamente llamada _link-local address_ tiene los mismos valores en la porción de host. Eso es porque ambos usan _Modified EUI-64_.

El proceso para generar el identificador de la interface de 64 bits es:
1. Divide la MAC address (48 bits) por la mitad 
2. Inserta el valor `0xfffe` en el medio
3. Invierte el valor del 7mo bit binario 

![[Pasted image 20241209021231.png]]

**Example Modified EUI-64 interface IDs**

![[Pasted image 20241209021435.png]]


