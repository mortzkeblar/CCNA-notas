---
tags:
  - dynamic
  - OSPF
  - routing
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

## backbone
Todo el trafico debe pasar a través del backbone, y todas las areas deben estar conectadas a el area 0. El area 0 no puede ser particionado (es decir, debe ser continua), puedes ampliar el uso del area 0 con virtual links. 