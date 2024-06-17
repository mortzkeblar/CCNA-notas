---
tags:
  - routing
  - CCNA
---

Recordad que la tabla de enrutamiento o Routing Information Base (RIB) contiene la información para poder reenviar el trafico (en caso de que las haya). La tabla por si mismo, no reenvía el trafico.

Los routers Cisco usan [administrative distance](administrative%20distance.md) , [metrics](metrics.md)  y/o el [longest prefix matching](longest%20prefix%20matching.md)  para determinar cual ruta deberá ser agregada a la tabla de enrutamiento. 
- Si la ruta entrante no existe actualmente en la tabla, se añade a la tabla de enrutamiento.
- Si la ruta entrante es más especifica que otra ya existente en la tabla, se añade a la tabla. Notar que la ruta anterior aun permanece en la tabla.
- Si la ruta entrante es la misma que otra ya existente pero proviene de una ruta de origin preferente, esta reemplaza la viaja entrada.
- Si la ruta entrante es la misma que otra ya existente y ambos tiene el mismo protocolo:
	- Se descarta la nueva ruta si la [Metric](metric.md) es más alta que la ruta existente.
	- Reemplaza la ruta existente si la [metric](metric.md) de la nueva ruta es menor
	- Si la [metric](metric.md) para ambos es la misma, usa ambas rutas para load balancing (balanceo de carga)


Cuando se construye RIB por defecto, el protocolo de routing con la distancia administrativa más baja siempre gana cuando el router determina cuales rutas permite ingresar a la routing table.
P. ej, si el router recibe 10.0.0.0/8 via external EIGRP, OSPF and internal BGP. La ruta OSPF sera acomodado dentro de la routing table. Si la ruta es removida o no es más larga recibida,la ruta external EIGRP la reemplazara en la tabla, y así sucesivamente con BGP.

Una vez que la rutas esten acomodadas en la routing table, por defecto, el prefix más especifico o el [longest prefix matching](longest%20prefix%20matching.md) siempre sera preferido sobre rutas menos especificas.
## connected 
[best routing path selection criteria](best%20routing%20path%20selection%20criteria.md) 