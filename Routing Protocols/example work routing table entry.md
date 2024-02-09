# example work routing table entry
Ver: [best routing path selection criteria](best%20routing%20path%20selection%20criteria.md) 
Cuando se construye RIB por defecto, el protocolo de routing con la distancia administrativa más baja siempre gana cuando el router determina cuales rutas permite ingresar a la routing table.
P. ej, si el router recibe 10.0.0.0/8 via external EIGRP, OSPF and internal BGP. La ruta OSPF sera acomodado dentro de la routing table. Si la ruta es removida o no es más larga recibida,la ruta external EIGRP la reemplazara en la tabla, y así sucesivamente con BGP.

Una vez que la rutas esten acomodadas en la routing table, por defecto, el prefix más especifico o el [longest prefix matching](longest%20prefix%20matching.md) siempre sera preferido sobre rutas menos especificas.