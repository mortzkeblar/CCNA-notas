---
tags:
  - dynamic
  - routing
  - OSPF
  - CCNA
aliases:
  - Link State Advertisements
date created: Saturday, October 19th 2024, 11:08:14 pm
date modified: Wednesday, November 20th 2024, 11:33:50 pm
---

[OSPF](OSPF.md) usa LSAs (Link State Advertisements) para notificar a los vecinos sobre camibiso en la red. Los LSA son usados para construir la OSPF database. 

Si bien existen varios tipos de LSAs, solo vamos a hablar sobre aquellos que se cubren en la certificación. 

Algunos de los LSA más comunes incluyen:
- **[LSA Type 1](LSA%20Type%201.md) (Router LSA)** - generado por todos los routers, esta LSA describe los enlaces de los routers 
- **[LSA Type 2](LSA%20Type%202.md) (Network LSA)** - Generado por el DR en una _broadcast network type_, esta LSA lista todas los routers en el segmento
- [LSA Type 3](LSA%20Type%203.md) (Summary LSA)
- [LSA Type 4](LSA%20Type%204.md) (ASBR summary LSA)
- **[LSA Type 5](LSA%20Type%205.md) (AS External LSA)** - Generado por los routers ASBR, esta LSA anuncia rutas a redes externas (i.e [[Default route]] a internet).

![[Pasted image 20241204010545.png]]

> #exam Los LSA Types ya no son parte del examen de certificación 

Se puede ver el contenido de la **Link State Database** usando `show ip ospf database`. 

![[Pasted image 20241204010625.png]]

## LSA Type 1

Tambien llamado **Router Link State Advertisements**, los LSA Type 1 son generados por cada tipo de router (backbone, stud, NSSA, non-stub, etc). 
- Los LSA enumeran el [(legacy) RID]((legacy)%20RID.md) del router de origen
- Contienen los enlaces conectados directamente en esa area
- Para cada tipo de enlace, el LSA contiene un link ID y un ADV router, que es el que se anuncia como [(legacy) RID]((legacy)%20RID.md). 

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

## LSA Type 2 

**Network Link State Advertisements**, es usado para anunciar routers en segmentos multi-access. Esta LSA es originada por el [DR]((LEGACY)%20DR%20and%20BDR.md) y hacen flooding dentro del area, al igual que [LSA Type 1](LSA%20Type%201.md). 

El LSA Type 2 incluye el link ID (el cual es la dirección del DR), la mascara y los [(legacy) RID]((legacy)%20RID.md)s de los routers en la red (routers conectados). 

``` bash
R3#show ip ospf database network
OSPF Router with ID (3.3.3.3) (Process ID 3)
Net Link States (Area 0)
Routing Bit Set on this LSA
LS age: 248
Options: (No TOS-capability, DC)
LS Type: Network Links
Link State ID: 10.0.1.2 (address of Designated Router)
Advertising Router: 2.2.2.2
LS Seq Number: 80000008
Checksum: 0x8E7B
Length: 32
Network Mask: /24
Attached Router: 2.2.2.2
Attached Router: 3.3.3.3
```

## LSA Type 3

**Summary Link State Advertisements**, proporciona información sobre destinos fuera del area local (inter-area routing information). A diferencia de las [LSA Type 1](LSA%20Type%201.md), las LSA Type 3 no brindan información de la topologia, sino que resumente la información topologica recibida de las [LSA Type 1](LSA%20Type%201.md) y [LSA Type 2](LSA%20Type%202.md) en un prefijo y su coste asociado. 

Estas LSA son generadas por el ABR, el proceso de flooding es:
- LSAs Type 3 son anunciadas desde un area non-backbone al area backbone (0) para cada ruta intra-area (LSA Type 1 y 2). 
- LSAs Type 3 son inundados desde el area 0 a otras areas non-backbone para cada ruta intra-area dentro del area 0 y para las rutas inter-area ([LSA Type 2](LSA%20Type%202.md) que se reciben de otras areas). 

![](16-6-scaled%202.jpg)

Con `show ip ospf database summary [options]` podemos ver los LSA resumidos en la DB de estados de enlace (LSDB). Podemos ver información sobre: 
- Link ID
- Subnet mask
- [OSPF cost](OSPF%20cost.md) 

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

La [metric]((OLD)%20metric.md) de la ruta en la salida del comando es la distancia desde el ABR. El router receptor añade su propia métrica de interface para determinar la métrica global de la ruta. Importante considerar esto para evitar confusiones el revisar el LSDB.

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

## LSA Type 4

Estos LSA son generados por el ABR y son usados para brindar información sobre como encontrar un ASBR. 

Se diferencia de [LSA Type 3](LSA%20Type%203.md) porque este proporciona información de como encontrar profijos en un area para otra area.

Para que se puede generar un LSA Type 4, debe haber un ASBR en el area. Un router ASBR suele inyectar rutas de otro dominio dentro de OSPF via redistribución. 

## LSA Type 5 

**External Summary Link State Advertisements**, estos proporcionan información sobre destinos fuera del dominio [OSPF](OSPF.md), son rutas aprendidas por estos routers desde otras fuentes de enrutamiento fuera de OSPF. 

Los LSA Type 5 son generados por routers ASBR (un router que conecta un dominio OSPF con otra fuente de enrutamiento) y por defecto, hacen flooding en todo el dominio. 

