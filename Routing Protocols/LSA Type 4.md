---
tags:
  - dynamic
  - routing
  - OSPF
---
Estos LSA son generados por el ABR y son usados para brindar información sobre como encontrar un ASBR. 

Se diferencia de [LSA Type 3](LSA%20Type%203.md) porque este proporciona información de como encontrar profijos en un area para otra area.

Para que se puede generar un LSA Type 4, debe haber un ASBR en el area. Un router ASBR suele inyectar rutas de otro dominio dentro de OSPF via redistribución. 

