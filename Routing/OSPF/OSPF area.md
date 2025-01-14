---
tags:
  - dynamic
  - OSPF
  - routing
  - CCNA
date created: Saturday, October 19th 2024, 11:08:14 pm
date modified: Wednesday, November 20th 2024, 11:33:50 pm
---

[OSPF](OSPF.md) usa el concepto de areas, los cuales son grupos lógicos asociados a las interfaces de un router. Dentro de un área en OSPF, solo se mantiene información detallada sobre los enlaces y redes pertenecientes a esa área, reduciendo la propagación de datos de enrutamiento hacia otras áreas

> OSPF area tambien puede ser definido como el conjunto de routers que comparten el mismo [[OSPF#Link-State Database]]

Esto en parte ayuda a que no el CPU no se sobrecargue debido al proceso OSPF como flooding, mantenimiento de la base de datos o el consumo de ancho de banda.
- Reducen el tamaño de la _Link State Database_, menor imparto en la memoria y procesador del router.
- [LSA](LSA.md) flooding  esta limitado dentro del area, reduciendo así el ancho de banda.

[[OSPF]] clasifica las áreas en diferentes tipos según los [[LSA|Link State Advertisements]] que se permiten en cada una. estas incluyen:
- Backbone (Area 0)
- Stub areas
- Totally stubby areas 
- Not-so-stubby areas 
- Totally non-so-stubby areas 
- Non-backbone, non-stub areas 

Estas areas se basan en una estructura _two-level hierarchical_ entre el _backbone_ area y los _non-backbone_ areas. Todas las _non-backbone_ areas deben estar conectadas con el _backbone_ area, y todos el trafico entre areas debe pasar por el _backbone_ area. 

![[Pasted image 20241120235545.png]]

### OSPF router types
OSPF tambien distigue diferentes tipos de routers dependiente su ubicación entre las areas, a saber:
- _Internal Router_ - todas las interfaces OSPF estan habilitadas para el mismo area 
- _Backbone Router_ - tienen al menos una de sus interfaces esta habilitada en el Area 0
- _Area Border Router (ABR)_ - routers que conectan una (o más) areas al Area 0
	- Esto se debe a que las areas _non-backbone_ no pueden estar conectadas entre sí, sin pasar antes por el _backbone_ area
- _Autonomous System Boundary Router (ASBR)_ - routers que conectan el _OSPF Autonomous System_ a redes externas (i.e. internet)

### OSPF route types
Otra clasificación que realiza [[OSPF]] es sobre las rutas:
- _Intra-area route_ - son rutas con destinos que se encuentran en el mismo area que el router 
- _Inter-area route_ - son rutas a destinos que están en un area en la que el router no tiene interfaces conectadas a esa area 
- _External route_ - rutas a destinos externos, son anunciadas por el ASBR router 

![400](16-5-scaled.jpg)
## Backbone Area
Todo el trafico debe pasar a través del backbone, y todas las areas deben estar conectadas a el area 0. El area 0 no puede ser particionado (es decir, debe ser continua), aunque se saltar esta limitación del area 0 con [[OSPF virtual links]]. 

## Stub Area 
Las stub areas no permiten [[LSA]]s externos, este esta area, los [[LSA Type 5]] no se permiten dentro del area. 
> Significa que el [[LSA Type 5]] que se originan de otras areas son filtradas por el ABR para que no ingresen al stub area. Tambien implica que este area no hay ASBR. 
> Si se usan [[LSA Type 4]] para describir un ASBR, estos tambien no son permitidos en el stub area. 

Esto asegura que el stub area puede encontrar destinos externos desde otras areas, el ABR inyecta una _summary default route_ dentro del stub area. 


![[Pasted image 20240906170854.png]]

``` bash
R1(config)#router ospf 1
R1(config-router)#area 1 stub
R1(config-router)#exit
R2(config)#router ospf 2
R2(config-router)#area 1 stub
R2(config-router)#exit


```
