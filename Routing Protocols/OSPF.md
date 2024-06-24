---
tags:
  - routing
  - dynamic
  - OSPF
  - protocol
  - CCNA
---

Open Shortest Path First (OSPF protocol) es un protocolo estándar abierto que usa algoritmos _link state_ para calcular el mejor camino para llegar a una red particular. Desarrollado en el '98 por IETF para cubrir las necesidades y las carencias del [RIP](RIP.md). Es un protocolo más robusto y flexible que los predecesores [distance vector protocol](distance%20vector%20protocol.md) y es ideal para usarlo en entornos enterprise. 

OSPF emplea el uso de _areas_ para simplificar la administración de la red y confinar la inestabilidad de la red a un sector especifico. Además permite un amplio control sobre las actualizaciones de enrutamiento.

OSPF usa Dijkstra's Shortest Path First (SPF) algorithm, eso hace referencia a que es abierto ya que este algoritmo no es propietario a ninguna empresa o organización.

OSPF se considera un _Interior Gateway Protocol ([IGPs](IGPs.md))_ porque es un protocolo que puede ser usado en grupos de routers que se encuentran dentro de un [autonomous system](autonomous%20system.md). 

Tiene una [administrative distance](administrative%20distance.md) de 110.

Además de mejorar los metodos de protocolos anteriores, introduce caracteristicas como:
- No limitaciones en hop-count
- Rapida convergencia
- [classless](classless.md)  (permite el uso de [VLSM](../VLSM.md))
- Autenticación de contraseñas
- Metodos avanzados de selección de rutas
- Etiquetado para rutas externas
- Mejor uso del ancho de banda via multicast y actualizaciones de routing periódicas.
- Permite a las redes ser divididas dentro de areas lógicas más pequeñas para la eficiencia.
- Usa direcciones multicast por eficiencia y fiabilidad en las actualizaciones de enrutamiento.
- Usa equal-cost load balancing sobre multiples rutas para optimizar el uso de ancho de banda. 
- Soporta MD5 authentication para el intercambio seguro de rutas.
- No tiene problemas de [split horizon](split%20horizon.md). 


## connected

- [OSPF terminology](OSPF%20terminology.md)
- [OSPF overview](OSPF%20overview.md) 
- [OSPF explain summary - advanced](OSPF%20explain%20summary%20-%20advanced.md) 
- [OSPF network types and neighbors](OSPF%20network%20types%20and%20neighbors.md) 
- [OSPF routes](OSPF%20routes.md) 
- [OSPF router types](OSPF%20router%20types.md) 
- [LSA](LSA.md) 

#TODO ver este contenido y resumir: https://itskillbuilding.com/networking/network/ospf/ospf-protocol/#tab-3711