---
tags:
  - routing
  - dynamic
  - CCNA
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
- Link State Protocols 
- Distance Vector Protocols
- Path Vector Protocols 

![[Pasted image 20241118150728.png]]

## Route selection 
La selección de rutas ya fue definida anteriormente (ver [[Routing Table]]), esta hace referencia al reenvio de paquetes en base la ruta coincidente más especifica que haya dentro del tabla de enrutamiento.

_Route selection_ tambien hace referencia al proceso de selección de rutas que terminan agregandose en la [[Routing Table]]. Por ejemplo si tenemos multiples rutas para llegar a un mismo destino, solo se agregara a la tabla de enrutamiento la mejor ruta para llegar a ese destino. 

Para determinar la mejor ruta para llegar a un determinado destino, se utilizan dos parametros.
- Metric
- Administrative distance 

### Metric parameter 
Este concepto se utiliza cuando se tiene multiples rutas para llegar al mismo destino. Las métricas se ejecutan sobre esas rutas aprendidas para elegir el mejor camino posible según un sistema de clasificación que permite dar preferencia a unas rutas sobre otras y estas sean agregadas en la tabla de enrutamiento.

Cada protocolo de enrutamiento calcula su metrica de forma diferente.

**IGP metrics**
![[Pasted image 20241118182036.png]]

En la siguiente imagen, se tiene unejemplo de selección de rutas usando [[OSPF]].
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
*Equal-Cost Multi-Path (ECMP)* routing es una 