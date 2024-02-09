Variable Length Subnet Masks (VLSM) es un metodo [classless](Routing%20Protocols/classless.md) que surgio como una evolución de FLSM. 

El concepto básico de VLSM es muy simple: se toma una red y se divide en subredes fijas, luego se toma una de esas [subredes](https://es.wikipedia.org/wiki/Subred "Subred") y se vuelve a dividir, tomando bits "prestados" de la porción de hosts, ajustándose a la cantidad de hosts requeridos por cada segmento de nuestra red.

Algunos de los protocolos que soportan [VLSM]() son [RIPv2](Routing/Dynamic/RIPv2.md), OSPF y las versiones más recientes de BGP y EIGRP.


### some examples 
Su ISP le asigno esta red `/24`: `200.2.2.0/24`

Tienes un router y la lista de requerimientos de direcciones IP a asignar. 
- Fa0/1 - Needs 20 IP addresses - Assigned: 200.2.2.0/27
- Fa0/3 - Needs 15 IP addresses - Assigned: 200.2.2.32/27
- Fa0/2 - Needs 40 IP addresses - Assigned: 200.2.2.64/26
- Fa0/0 - Needs 10 IP addresses - Assigned: 200.2.2.128/28

La idea es tratar de asignar el bloque IP con el valor de subred más cercano a los requerimientos necesarios. No siempre se logra un valor exacto, pero es mucho más eficiente que FLSM. 