Este concepto hace referencia a la toma de decisión a la hora de que el router tenga que hacer forwarding de un paquete en base a la [[routing table]]. Para esto se basa en el _most specific matching route_, que se puede definir como:
- Matching route - El IP de destino del paquete es parte de la red especificada en la ruta 
- Most specific - la ruta con el _longest prefix matching_. 

![[Pasted image 20241029084043.png|500]]
> Una cuestión a considerar entre las diferencias de capa 2 y capa 3. Es que cuando un router mira la dirección de destino trata de hace coincidir con la ruta más especifica para llegar al destino. En cambio en capa 2 la coincidencia tiene que ser exacta, no existe la busqueda parcial. 
> Otra diferencia radica en que cuando un router no encuentra un destino para el paquete este se descarta, en cambio un switch realiza un flood cuando recibe un unknown unicast frame. 

![[Pasted image 20241031231700.png]]
### Longest prefix matching
Esta regla sirve para determinar que rutas se deberían usar para enviar un paquete hacia su destino. Indica que un router siempre va a preferir la entrada de la tabla de routing más larga o especifica a las entradas menos especificas (p. ej. summary addresses), al determinar que entrada usar para reenviar el trafico.

| Routing Table Entry | Order Used |
| ------------------- | ---------- |
| 1.1.1.1/32          | 1st        |
| 1.1.1.0/24          | 2nd        |
| 1.1.0.0/16          | 3rd        |
| 1.0.0.0/8           | 4th        |
| 0.0.0.0/0           | 5th        |