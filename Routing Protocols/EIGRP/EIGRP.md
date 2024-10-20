Enhanced Interior Gateway Routing Protocol (EIGRP) es un [[dynamic routing protocols]] de tipo hibrido, es decir combina las caracteristicas de los protocolos basados en [[distance vector protocols]] y los [[link state protocols]]. 

EIGRP tiene varias ventajas como soporte VLSM, baja uso de CPU, se adapta bien en grandes entornos y tiene soporte para autenticación MD5. Tambien es el unico protocolo que soporta multiples protocolos de enrutamiento. 

Se le considera como un protocolo [[classless]], usa _Diffusing Update Algorithm ([[DUAL]])_ para calcular las rutas loop-free en base a la información recopilada. Esta rastrea todas las rutas anunciadas por los routers vecinos y selecciona la mejor ruta basándose en métricas que consideran el ancho de banda, delay, confiabilidad y carga. 

EIGRP realiza la summarization en subredes mayores, si se utiliza [[VLSM]] en la red, se debe desactivar la summarization con `no auto-summary` para no ocasionar problemas a la hora de publicar nuestras rutas.

## terminology
- Metric - el calculo de la metrica en EIGRP es a través de un algorimo compuesto por el bandwidth, delay (of the line), reliability and load. EIGRP usa el bandwidth y delay por defecto $$metric=(bandwidth\ +\ delay) \cdot 256$$
- Topology table - esta se compone de todas las rutas aprendidas via EIGRP, cada router mantiene una tabla, la cual contien las metricas y distancias factibles asociadas con las rutas. 
- Neighbor table - lista de routers adyacentes corriendo EIGRP, se puede ver esta tabla con `show ip eigrp neighbors`
- Successor/feasible successor - cuando EIGRP ejecuta el algoritmo DUAL, parte de ese proceso consiste en determinar un successor y un feasible successor para cada ruta en la EIGRP routing table. 
	- El successor es la camino principal para determinada ruta 
	- El feasible successor es el siguiente mejor camino principal para determinada ruta, este actua como backup en caso de que el camino principal no este disponible 
- Internal route - son rutas originadas dentro de un EIGPR AS ([[autonomous system]])
- External route - son rutas aprendidas desde otro protocolo de enrutamiento o [[autonomous system]], estas se muestran en la routing table como rutas estaticas