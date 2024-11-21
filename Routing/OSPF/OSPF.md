---
tags:
  - routing
  - dynamic
  - OSPF
  - protocol
  - CCNA
---

Open Shortest Path First (OSPF protocol) es un protocolo estándar abierto que usa algoritmos _link state_ (ver [[Interior Gateway Protocols]]) para calcular el mejor camino para llegar a una red particular. 

Desarrollado en el '98 por IETF para cubrir las necesidades y las carencias del [RIP](RIP.md). Es un protocolo más robusto y flexible que los predecesores protocolos de tipo [[Interior Gateway Protocols#Distance-Vector Protocols]] y es ideal para usarlo en entornos enterprise. 

OSPF emplea el uso de _areas_ para simplificar la administración de la red y confinar la inestabilidad de la red a un sector especifico. Además permite un amplio control sobre las actualizaciones de enrutamiento.

OSPF usa Dijkstra's Shortest Path First (SPF) algorithm, hace referencia a que es abierto ya que este algoritmo no es propietario a ninguna empresa o organización.

OSPF se considera un [[Interior Gateway Protocols]] porque es un protocolo que puede ser usado en grupos de routers que se encuentran dentro de un [AS](AS.md). 

Tiene una  [[Dynamic Routing#Administrative Distance parameter]] de 110.

Además de mejorar los metodos de protocolos anteriores, introduce caracteristicas como:
- No limitaciones en hop-count
- Rapida convergencia
- [classless](classless.md)  (permite el uso de [VLSM](VLSM.md))
- Autenticación de contraseñas
- Metodos avanzados de selección de rutas
- Etiquetado para rutas externas
- Mejor uso del ancho de banda via multicast y actualizaciones de routing periódicas.
- Permite a las redes ser divididas dentro de areas lógicas más pequeñas para la eficiencia.
- Usa direcciones multicast por eficiencia y fiabilidad en las actualizaciones de enrutamiento.
- Usa equal-cost load balancing sobre multiples rutas para optimizar el uso de ancho de banda. 
- Soporta MD5 authentication para el intercambio seguro de rutas.
- No tiene problemas de [split horizon](split%20horizon.md). 

## OSPF foundations 

> Los tres pasos fundamentales que siguen los routers OSPF para compartir la información y construir sus tablas de enrutamiento se pueden definir en:
> - Formar relaciones entre vecinos (o _neighbor relationships_) con otros routers OSPF-enabled 
> - Intercambiar información de enrutamiento para construir la _map connectivity_ de la red 
> - Calcular las mejores rutas a cada destino


[OSPF](OSPF.md) es un protocolo [[classless]] que establece relaciones con routers vecinos mediante paquetes multicast _Hello_ y temporizadores _Dead_. Las actualizaciones se realizan a través de _data structures_ llamadas _[[LSA|Link State Advertisements]]_. Estas LSA luego son organizadas en la **_Link State Database_** que actua como la tabla de topología de OSPF. Los LSAs se propagan por las áreas [OSPF](OSPF.md) hasta que todos los routers tienen un mapa consistente de la red (es decir, las bases de datos coinciden).

Cuando el mapa es consistente, el algoritmo SPF calcula rutas _loop-free_ (sin bucles) para cada red de destino, generando un _SPF tree_ que se refleja en la tabla de enrutamiento.

[OSPF](OSPF.md) utiliza una métrica basada en costo para determinar el camino más corto entre dos puntos.

1. Los routers OSPF envían paquetes _Hello_ por todas sus interfaces configuradas. Si los routers en el enlace los aceptan, se convierten en vecinos.
2. Algunos vecinos forman adyacencias, según el tipo de router y red (p. ej., _point-to-point_ o _broadcast_).
3. Los _Link State Advertisements_ se intercambian entre adyacencias, proporcionando información sobre enlaces, vecinos y su estado.
4. Cada router construye una base de datos _link state_ idéntica.
5. Una vez sincronizados, el algoritmo SPF calcula las mejores rutas _loop-free_ para cada destino, considerando a cada router como la raíz (_Root SPF_).
6. Finalmente, cada router genera su tabla de enrutamiento.

## Link-State Database 
En OSPF, el intercambio de información de enrutamiento se hace mediante el uso de una _data structure_ llamada [[LSA|Link State Advertisements]] (LSA). Cada router organiza los [[LSA]] que recibe en una base de datos llamada **_Link State Database (LSDB)_**.

LSDB sirve como el mapa de la topologia de la red al router, que es utilizado para calcular el _shortest path_ a cada destino de la red. 

![[Pasted image 20241120002451.png]]



## connected

- [OSPF terminology](OSPF%20terminology.md)
- [OSPF explain summary - advanced](OSPF%20explain%20summary%20-%20advanced.md) 
- [OSPF network types and neighbors](OSPF%20network%20types%20and%20neighbors.md) 
- [OSPF routes](OSPF%20routes.md) 
- [OSPF router types](OSPF%20router%20types.md) 
- [LSA](LSA.md) 

#TODO ver este contenido y resumir: https://itskillbuilding.com/networking/network/ospf/ospf-protocol/#tab-3711