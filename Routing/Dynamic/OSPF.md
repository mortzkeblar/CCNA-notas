Open Shortest Path First (OSPF protocol) es un protocolo estandár abierto que usa algoritmos _link state_ para calcular el mejor camino para llegar a una red particular. Desarrollado en el '98 por IETF para cubrir las necesidades y las carencias del [RIP](../RIP.md). Es un protocolo más robusto y flexible que los predecesores [distance vector protocols](distance%20vector%20protocols.md) y es ideal para usarlo en entornos enterprise. 

OSPF emplea el uso de _areas_ para simplificar la administración de la red y confinar la inestabilidad de la red a un sector especifico. Además permite un amplio control sobre las actualizaciones de enrutamiento.

OSPF usa Dijkstra's Shortest Path Firts (SPF) algorithm, eso hace referencia a que es abierto (Open) ya que este algoritmo no es propietario a ninguna empresa o organización.

Tiene una [administrative distances](administrative%20distances.md) de 110.

Además de mejorar los metodos de protocolos anteriores, introduce caracteristicas como:
- No limitaciones en hop-count
- Rapida convergencia
- [classless](../classless.md) (permite el uso de [VLSM](../../../../../VLSM.md))
- Autenticación de contraseña
- Metodos avanzados de selección de rutas
- Etiquetado para rutas externas
- Mejor uso del ancho de banda (bandwidth) via multicast y actualizaciones de routing periodicas.
- Permite a las redes ser divididas dentro de areas logicas más pequeñas para la eficiencia.
- Usa direcciones multicast por eficiencia y fiabilidad en las actualizaciones de enrutamiento.
- Usa equal-cost load balancing sobre multiples rutas para optimizar el uso de ancho de banda. 
- Soporta MD5 authentication para el intercambio seguro de rutas.
- No tiene problemas de [split horizon](split%20horizon.md). 