---
tags:
  - dynamic
  - routing
  - OSPF
  - CCNA
date created: Saturday, October 19th 2024, 11:08:14 pm
date modified: Wednesday, November 20th 2024, 11:33:50 pm
---
Tambien llamado **Router Link State Advertisements**, los LSA Type 1 son generados por cada tipo de router (backbone, stud, NSSA, non-stub, etc). 
- Los LSA enumeran el [RID](RID.md) del router de origen
- Contienen los enlaces conectados directamente en esa area
- Para cada tipo de enlace, el LSA contiene un link ID y un ADV router, que es el que se anuncia como [RID](RID.md). 

Cada router en una area envia LSA Type 1 a toda el area, si el router esta en varias areas, genera un LSA Type 1 para cada area al que esta conectado. 

![](16-6-scaled%201.jpg)

En el ejemplo de arriba R2 y R3 deben generar LSA Type 1 para el area 0.

``` bash
R3#show ip ospf database
OSPF Router with ID (3.3.3.3) (Process ID 3)
Router Link States (Area 0)
Link ID   ADV Router    Age        Seq# Checksum Link Count
2.2.2.2   2.2.2.2 704   0x80000005 0x0048A2 2
3.3.3.3   3.3.3.3 424   0x80000004 0x003AA4 2
[output truncated]
```

- _Age_ -  es la edad maxima del enlace. 
- _Seq#_/_Checksum_ - verifican la integridad del estado del enlace. 

Recordar que que la flooding (inundación) de LSA al ser de tipo 1 es solo dentro del area, no haran flooding en el area siguiente. 