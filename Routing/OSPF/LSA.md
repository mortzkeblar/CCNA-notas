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
- [LSA Type 1](LSA%20Type%201.md) (Router LSA)
- [LSA Type 2](LSA%20Type%202.md) (Network LSA)
- [LSA Type 3](LSA%20Type%203.md) (Summary LSA)
- [LSA Type 4](LSA%20Type%204.md) (ASBR summary LSA)
- [LSA Type 5](LSA%20Type%205.md) (External summary LSA)

> #exam Los LSA Types ya no son parte del examen de certificación 

Se puede ver el contenido de la **Link State Database** usando `show ip ospf database`. 

``` 
R1#show ip ospf database ?
adv-router Advertising Router link states
asbr-summary ASBR Summary link states
database-summary Summary of database
external External link states
network Network link states
nssa-external NSSA External link states
opaque-area Opaque Area link states
opaque-as Opaque AS link states
opaque-link Opaque Link-Local link states
router Router link states
self-originate Self-originated link states
summary Network Summary link states
| Output modifiers
[cr]
```

