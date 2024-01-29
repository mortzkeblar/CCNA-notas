Este concepto se utiliza cuando tenemos un caso de multiples rutas aprendidas para llegar al mismo destino. Las metrics se ejecutan sobre esas rutas aprendidas para elejir el mejor camino posible según un sistema de clasificación que permite dar preferencia a unas rutas sobre otras y estas sean agregadas en la tabla de enrutamiento.

> P. ej. RIP usa hop count, EIGRP usa el bandwidth más bajo y el delay total, tambien crea una tabla de mejores next-hop IP en caso de que la ruta principal falle.  

Las metricas usadas por los protocolos incluyen:
- Bandwidth - se elije segun la velocidad de enlace más rapida.
- Hop count - un hop es otro router, entonces tres router equivalen a tres hops.
- Load - la cantidad de trafico usado por el enlace, un enlace con la carga más baja es mejor. 
- Delay - la cantidad de tiempo que toma un paquete en atravesar un camino.
- Reliability - definido por la probabilidad de que un enlace falle debido a errores o reinicios en la interface.
- Cost - no hay una definición fija en este valor, refleja una ruta más o menos preferida

Esta son algunas de las metricas definidas. Dependiendo el protocolo, este puede usar uno a más metricas para establecer la mejor ruta posible. 