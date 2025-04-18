---
tags:
  - NAT
  - CCNA
date created: Saturday, October 19th 2024, 11:08:14 pm
date modified: Wednesday, November 20th 2024, 11:33:50 pm
---

# Network Address Translation
NAT es un proceso (diseñado específicamente para IPv4 y el problema de la falta de direcciones) de cambio de las direcciones IP y los puertos de origen/destino. La traducción de direcciones reduce la necesidad de direcciones publicas IPv4 y oculta rangos de direcciones privadas (definidos en [(LEGACY) NAT - RFC 1918]((LEGACY)%20NAT%20-%20RFC%201918.md)). De este proceso generalmente se encargan los routers o firewalls. 

#TODO Agregar una entrada sobre el agotamiento actual de las direcciones IPv4

> NAT, VLSM y CIDR son tres técnicas ampliamente utilizadas para prolongar el uso de direcciones IPv4 en internet.
> 
> Sin embargo NAT trajo limitaciones como la conexión end-to-end, etc.

### Tipos de NAT
- Dynamic NAT (_Ver: [(LEGACY) NAT - Dynamic]((LEGACY)%20NAT%20-%20Dynamic.md)_)
- Static NAT (_Ver: [(LEGACY) NAT - Static]((LEGACY)%20NAT%20-%20Static.md)_)
- Overloaded NAT/PAT (_Ver: [(LEGACY) PAT]((LEGACY)%20PAT.md)_)
- Dynamic NAT with overload (_Ver: [(LEGACY) NAT - Overload Dynamic (Multiple IP address)]((LEGACY)%20NAT%20-%20Overload%20Dynamic%20(Multiple%20IP%20address).md)_)
- NAT-T (NAT Traversal)
- Source NAT
- NAT64


#TODO Agregar una entrada desmintiendo la falsa seguridad y aislamiento que supuestamente proporciona NAT

### References
- https://www.ccnablog.com/nat-network-address-translation/
- https://ccnadesdecero.es/nat-network-address-translation/