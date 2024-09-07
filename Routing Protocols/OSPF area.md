---
tags:
  - dynamic
  - OSPF
  - routing
  - CCNA
---

[OSPF](OSPF.md) usa el concepto de areas, los cuales son grupos lógicos asociados a las interfaces de un router. Los routers dentro de un área no mantendrán información detallada sobre los routers que estén fuera de su area específica.  

Esto en parte ayuda a que no el CPU no se sobrecargue debido al proceso OSPF como flooding, mantenimiento de la base de datos o el consumo de ancho de banda.
- Reducen el tamaño de la _Link State Database_, menor imparto en la memoria y procesador del router.
- [LSA](LSA.md) flooding (inundación) esta limitado dentro del area, reduciendo así el ancho de banda.

La especificación OSPF define areas especiales basadas en los LSA Types que se permiten dichas area: 
- Backbone (area 0)
- Stub areas
- Totally stubby areas 
- Not-so-stubby areas 
- Totally non-so-stubby areas 
- Non-backbone, non-stub areas 

> #exam Las definiciones de estas areas a detalle, no se cubren en el examen de certificación. 

### backbone area
Todo el trafico debe pasar a través del backbone, y todas las areas deben estar conectadas a el area 0. El area 0 no puede ser particionado (es decir, debe ser continua), puedes ampliar el uso del area 0 con virtual links. 

### stub area 
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
