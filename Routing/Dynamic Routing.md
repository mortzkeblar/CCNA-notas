---
tags:
  - routing
  - dynamic
  - CCNA
date created: Saturday, October 19th 2024, 11:08:14 pm
date modified: Wednesday, November 20th 2024, 11:33:50 pm
---
El routing dinamico hace uso de los llamados _routing protocols_ para descubrir redes alcanzables y poder definir las mejores rutas para poder llegar hacia ellas.

![[Pasted image 20241118115125.png]]

> El intercambio de información entre routers es llamado tambien _advertisement_.

Algunas de las ventajas frente a [[Static Routing]] son:
- Adaptability - las rutas estaticas no tienen la capacidad de reaccionar frente a cambios en la red. El enrutamiento dinamico se adapta a los cambios en la red, insertando nuevas rutas de ser necesario en el caso por ejemplo de que una ruta ya no sea accesible. 
- Scalability - el enrutamiento dinamico permite la escalabilidad en grandes entornos de red, que de otra manera abria que configurar manualmente rutas estaticas lo cual no es practico. 

## Types of routing protocols 
Los protocolos de enrutamiento se dividen en dos categorias:
- **[[Interior Gateway Protocols]] (IGP)** - estos protocolos son usados para intercambiar información de enrutamiento dentro de los llamados _Autonomous System (AS)_ 
- **[[Exterior Gateway Protocols]] (EGP)** - estos protocolos son usados para intercambiar información de enrutamiento entre los diferentes _Autonomous Systems_. 

![[Pasted image 20241118150505.png]]

Además de esta categorización, los protocolos de enrutamiento tambien pueden ser categorizados por el _algorithm type_, que describe como los routers comparten la información y calculan las rutas.
- [[Interior Gateway Protocols#Link-State Protocols]]
- [[Interior Gateway Protocols#Distance-Vector Protocols]]
- Path Vector Protocols

![[Pasted image 20241118150728.png]]

## Route selection 
> Otro significado para _route selection_ ya fue definida anteriormente en [[Routing Table#Route selection]], esta hace referencia al reenvio de paquetes en base la ruta coincidente más especifica que haya dentro del tabla de enrutamiento.

En el contexto de enrutamiento dinamico, **_route selection_** tambien hace referencia al proceso de selección de rutas que terminan agregándose en la [[Routing Table]]. Por ejemplo si tenemos multiples rutas para llegar a un mismo destino, solo se agregara a la tabla de enrutamiento la mejor ruta para llegar a ese destino. 

Para determinar esa mejor ruta que sera agregada a la tabla de enrutamiento, se utilizan dos parametros.
- Metric
- Administrative distance 

> Es importante recordar que _route selection_ puede hacer referencia a:
> - _Routing table population_ - el proceso de seleccionar que rutas en el router ingresaran a la [[Routing Table]] 
> - _Packet forwarding_ - el proceso de seleccionar la mejor ruta en la [[Routing Table]] para reenviar un paquete particular

### Metric parameter 
Este concepto se utiliza cuando se tiene multiples rutas para llegar al mismo destino. Las métricas se ejecutan sobre esas rutas aprendidas para elegir el mejor camino posible según un sistema de clasificación que permite dar preferencia a unas rutas sobre otras y estas sean agregadas en la tabla de enrutamiento.

Cada protocolo de enrutamiento calcula su metrica de forma diferente.

**IGP metrics**
![[Pasted image 20241118182036.png]]

En la siguiente imagen, se tiene un ejemplo de selección de rutas usando [[OSPF]].
![[Pasted image 20241118182630.png]]
En este caso, R1 decide que R3 es la mejor ruta para llegar a la red 192.168.3.0/24 ya que tiene una la mejor metrica (menor costo), por lo cual se agrega a la [[Routing Table]].

![[Pasted image 20241118182954.png]]
Se puede observar que la ruta tiene los valores agregados `[110/2]`.
- El primer campo `110` representa la distancia administrativa 
- El segundo campo `2` es la métrica de la ruta 

En caso de que esta ruta por algún motivo caiga, el router detectara la ruta como invalida y agregara a su tabla la ruta alternativa via R2, ya que ahora es esta ruta la que tiene la mejor metrica. 

### Administrative Distance parameter 
Este parámetro cobra sentido cuando tenemos más de un protocolo de enrutamiento corriendo sobre la red. Debido a que las métricas que usa cada protocolo de enrutamiento son diferentes entre sí, no hay forma de comparar directamente para decidir la mejor ruta para un determinado destino. 

_Administrative Distance (AD)_ es un valor que indica la preferencia de un protocolo sobre otro. Un AD con valor más bajo indica que ese protocolo es más confiable o _trustworthy_, entonces es la ruta que se elige para llegar a un determinado destino. 

**Default AD values (lower value is better)**
![[Pasted image 20241118223547.png]]
En la siguiente imagen se puede ver el uso del _administrative distance_ para seleccionar que ruta ingresa en la [[Routing Table]]. 

![[Pasted image 20241118230058.png]]
Debido a que el AD de [[EIGRP]] es menor que la de [[OSPF]], este es seleccionada para ser agregada en la table de enrutamiento. 
![[Pasted image 20241118230228.png]]
- En la ruta agregada, se puede ver que el AD es de `90` y la metrica es de `3584`. SI bien puede parecer que la metrica de OSPF es mejor (en este caso tendria el valor de `4`) es irrelevante ya que en realidad esos costos no son comparables entre sí, es por eso que se hace uso del AD para determinar la mejor ruta. 

> Las metricas y el AD, solo se usa en caso de comparar rutas que dirigen al mismo destino, esto implica tanto que sea la misma _network address_ como el _prefix length_, si la network address es igual pero el prefix es diferente se consideran diferentes destinos, por lo cual cada red tiene su entreda independiente en la [[Routing Table]]. 

#### ECMP
*Equal-Cost Multi-Path (ECMP)* routing es una función que se encuentra en los routing protocols.

Esta aplica cuando existen multiples rutas para llegar a un destino, en la cual usan el mismo protocolo de enrutamiento y a la vez todas las rutas tienen el mismo valor de métrica. En este caso, todas las rutas son agregadas a la [[Routing Table]] sobre las cuales se realizara load-balancing del trafico que pase sobre ellas. 

> Por defecto, se puede realizar ECMP en un maximo de 4 rutas que van al mismo destino 

![[Pasted image 20241118234030.png]]

#### Floating static routes 
Una _floating static route_ es una ruta estática configurada con un AD mayor a 1 (valor por defecto), esto con el fin de que no tenga prioridad sobre la ruta principal (en este caso una ruta dinámica) pero que pueda servir de ruta de backup en caso de que la ruta dinámica falle.

Esto se logra con el comando `ip route <destination-network> <netmask> <next-hop> <ad-value>`.

![[Pasted image 20241118234558.png]]
Por ejemplo en caso de configurar una floating static route para una ruta OSPF, esta deberia tener un valor de AD mayor estricto a 90. 

Si bien un routing protocol puede realizar la misma función (tener un ruta de respaldo), la _floating static route_ permite tener una ruta de respaldo hacia un router que no este conectada a la red donde se ejecuta el protocolo de enrutamiento (en la imagen anterior, se agrega se genera un ruta de respaldo que van por routers que no ejecutan OSPF).

## The `network` command 
Los protocolos de enrutamiento dinamico como [[OSPF]], [[EIGRP]] y [[RIP]] se configuran activando el protocolo en una o más interfaces, luego el router _advertise_ el prefijo de red (network address and netmask) de la interface. 

El comando para realizar estas acciones es `network` (las cuales se usa en RIP, EIGRP y OSPF), este le dice al router que:
- Busque interfaces con una IP address que este en el rango especificado 
- Active el protocolo de enrutamiento en esas interfaces 
- Anuncie el _network prefix_ de las interfaces a sus vecinos 

Para configurar OSPF en un router Cisco se usa `router ospf <process-id>` en el modo _global config_ que lo llevara al modo _router config_ (un nuevo configuration mode) desde el cual puede usar `network`.

La sintaxis para usar `network` en un comando es `network <ip-address> <wild-card-mask> area <area-id>`

![[Pasted image 20241119021635.png]]

### Wildcard Masks
Una ***wildcard mask*** es una serie de 32 bits que indican cuales bits deben coincidir entre dos IP address y cuales no, La wildcard mask configurada con `network` indica los bits que deben coincidir entre la dirección IP usada en el comando `network` y la IP address asignada en la interface del router. 

En los siguientes ejemplo se puede ver el matching entre la dirección de red usanda en el comando `network` y la dirección IP configura en la interface, a la que se aplica OSPF. 
![[Pasted image 20241119034936.png]]
![[Pasted image 20241119035856.png]]

![[Pasted image 20241119040429.png]]
Los wildcard masks se los puede ver también como las _subnet mask_ pero de forma inversa, en este caso. La wildcard mask `0.0.0.3` es equivalente al netmask `/30 (255.255.255.252)` . 
![[Pasted image 20241119040254.png]]

**/24+ netmasks and wildcards masks**

![[Pasted image 20241119040516.png]]

> El motivo de usar las _wildcard mask_ es vez de una _subnet mask_ es que nos permite definir rangos arbitrarios de direcciones IP que queremos que pertenezca a [[OSPF]] (estas no necesariamente forman parte de la misma subred). 
> - Por ejemplo con una wildcard mask podemos definir algo como `network 192.168.0.0 0.0.5.255 area 0`
> 	- La inversa de `0.0.5.255` es `255.255.250.0`, hacemos la conversión a binario. 
> 		- Vemos que `255.255.250.0` no es un patron contiguo de 1s, por lo cual es invalido para su uso como subnet mask.
> 		- Con wildcard mask no tenemos esa limitación porque no exigen que los 1s sean contiguos #TODO revisar este concepto

La flexibilidad de la _wildcard mask_ nos permite activar OSPF en más de una interface siempre que los bits apropiados (0s) coincidan entre la IP usada en el comando `network` y la IP de la interface.

![[Pasted image 20241119042820.png]]
El primer comando activa [[OSPF]] en las interfaces G0/0 y G0/1, los bits apropiados coinciden entre la IP de `network` command y la IP de la interface, usando un wildcard de `0.0.0.7`, equivalente a un netmask `/29 (255.255.255.248)`. Le indica a R1 que active OSPF en todas las interfaces con la IP address entre `192.168.1.0 - 192.168.1.7`. 

![[Pasted image 20241119043206.png]]

El segundo comando activa OSPF, especificando la dirección IP exacta configurada en G0/2 con una wildcard mask de `0.0.0.0`. Le indica a R1 que debe activar OSPF solo en la interface con la IP address `192.168.2.1`

![[Pasted image 20241119043636.png]]

> Si bien ambas formas de usar `network` son validas, la recomendación es usar la segunda forma. Es decir, especificar la IP address exacta de la interface y usar una wildcard mask de `0.0.0.0`. Esto asegura que solo se activa las interfaces que uno desea. 

Es importante destacar tambien que al configurar las wildcard masks con `network` no se esta especificando su mascara de red, sino que especifica las interfaces que se deben activar porque la IP address configura en esa interface se encuentra dentro de un rango definido en la wildcard mask. Luego el router, se encarga de anunciar la ruta con su respectivo _network prefix_. 