---
tags:
  - dynamic
  - routing
  - OSPF
---
**Summary Link State Advertisements**, proporciona información sobre destinos fuera del area local (inter-area routing information). A diferencia de las [LSA Type 1](LSA%20Type%201.md), las LSA Type 3 no brindan información de la topologia, sino que resumente la información topologica recibida de las [LSA Type 1](LSA%20Type%201.md) y [LSA Type 2](LSA%20Type%202.md) en un prefijo y su coste asociado. 

Estas LSA son generadas por el ABR, el proceso de flooding es:
- LSAs Type 3 son anunciadas desde un area non-backbone al area backbone (0) para cada ruta intra-area (LSA Type 1 y 2). 
- LSAs Type 3 son inundados desde el area 0 a otras areas non-backbone para cada ruta intra-area dentro del area 0 y para las rutas inter-area ([LSA Type 2](LSA%20Type%202.md) que se reciben de otras areas). 

![](_anexos_/16-6-scaled%202.jpg)

Con `show ip ospf database summary [options]` podemos ver los LSA resumidos en la DB de estados de enlace (LSDB). Podemos ver información sobre: 
- Link ID
- Subnet mask
- [cost](cost.md) 

``` bash
R4#show ip ospf database summary
OSPF Router with ID (4.4.4.4) (Process ID 4)
Summary Net Link States (Area 2)
Routing Bit Set on this LSA
LS age: 1612
Options: (No TOS-capability, DC, Upward)
LS Type: Summary Links(Network)
Link State ID: 1.1.1.1 (summary Network Number)
Advertising Router: 3.3.3.3
LS Seq Number: 80000001
Checksum: 0x9753
Length: 28
Network Mask: /32
TOS: 0 Metric: 66
Routing Bit Set on this LSA
LS age: 1612
Options: (No TOS-capability, DC, Upward)
LS Type: Summary Links(Network)
Link State ID: 2.2.2.2 (summary Network Number)
Advertising Router: 3.3.3.3
LS Seq Number: 80000001
Checksum: 0xE640
Length: 28
Network Mask: /32
TOS: 0 Metric: 2
Routing Bit Set on this LSA
LS age: 1677
Options: (No TOS-capability, DC, Upward)
LS Type: Summary Links(Network)
Link State ID: 3.3.3.3 (summary Network Number)
Advertising Router: 3.3.3.3
LS Seq Number: 80000001
Checksum: 0xAE75
Technet24.ir
Length: 28
Network Mask: /32
TOS: 0 Metric: 1
Routing Bit Set on this LSA
LS age: 1487
Options: (No TOS-capability, DC, Upward)
LS Type: Summary Links(Network)
Link State ID: 10.0.0.0 (summary Network Number)
Advertising Router: 3.3.3.3
LS Seq Number: 80000003
Checksum: 0x35AE
Length: 28
Network Mask: /24
TOS: 0 Metric: 65
[output truncated]
```

La [metrics](metrics.md) de la ruta en la salida del comando es la distancia desde el ABR. El router receptor añade su propia métrica de interface para determinar la métrica global de la ruta. Importante considerar esto para evitar confusiones el revisar el LSDB.

#TODO Ver si la etiqueta metrics corresponde en ese parrafo.

``` bash
R4#show ip ospf database summary 172.16.1.0
OSPF Router with ID (4.4.4.4) (Process ID 4)
Summary Net Link States (Area 2)
Routing Bit Set on this LSA
LS age: 76
Options: (No TOS-capability, DC, Upward)
LS Type: Summary Links(Network)
Link State ID: 172.16.1.0 (summary Network Number)
Advertising Router: 3.3.3.3
LS Seq Number: 80000002
Checksum: 0x8D99
Length: 28
Network Mask: /24
TOS: 0 Metric: 75
```