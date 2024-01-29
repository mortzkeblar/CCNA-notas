# Routing Information Protocol v2 (RFC 2453)
![](../../_anexos_/Screenshot%20from%202023-12-27%2018-32-39.png)

RIPv2 usa hop-count como metrica para encontrar el mejor camino para una ruta definida. Con un hop-count maximo de 15. RIPv2 envia mensajes multicast por defecto (se puede configurar para usar unicast o broadcast).
A diferencia de su antecesor RIPv1, este RIPv2 permite VLSM en la red al ser un protocolo classless.  

Hop-count se mide a través de los 'saltos' entre cada router hasta llegar al destino. `Menor cantidad de saltos = mejor`. 
- Es puede ser un problema por ejemplo, si el ancho de banda es más baja o la linea esta congestionada. Simplemente va a ir siempre por la ruta con la menor cantidad de saltos.

RIP (ambos protocolos) envía información de routing cada 30 segs, esto tiene como desventajas:
- En redes grandes, esto puede ser particularmente lento.
- En redes estables (es decir sin cambios en la topologia), RIP sigue enviando información de routing cada 30 segs, lo que hace que se desperdicie ancho de banda. 
### Laboratory
_Ver: [RIPv2 - Laboratory](RIPv2%20-%20Laboratory.md)_