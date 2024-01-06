# Network Address Translation
> NAT, VLSM y CIDR son tres técnicas ampliamente utilizadas para prolongar el uso de direcciones IPv4 en internet.
> 
> Sin embargo NAT trajo limitaciones como la conexión end-to-end, etc.

### Tipos de NAT
- Dynamic NAT (_Ver: [NAT - Dynamic](NAT%20-%20Dynamic.md)_)
- Static NAT (_Ver: [NAT - Static](NAT%20-%20Static.md)_)
- Overloaded NAT/PAT (_Ver: [NAT - Overload or PAT](NAT%20-%20Overload%20or%20PAT.md)_)
- Dynamic NAT with overload (_Ver: [NAT - Overload Dynamic (Multiple IP address)](NAT%20-%20Overload%20Dynamic%20(Multiple%20IP%20address).md)_)
- NAT-T (NAT Traversal)
- Source NAT
- NAT64


_En este punto es importante ver el rango de IP privadas establecidas en la RFC 1918 y el porque de seguir las buenas practicas [NAT - RFC 1918](NAT%20-%20RFC%201918.md)_

> Importante: en el examen de certificación es comun ver algun ejercicio relacionada a un mal funcionamiento de NAT. Por general suele ser con `inside/outside` mal configurado. Tomar enfasis en ese punto.


#### Formalidades en la syntaxis
- Red interna LAN (`inside`)
- Red externa WAN (`outside`)

### Misc
##### Inside/Outside NAT examples
_Ver: [NAT - Inside and Outside](NAT%20-%20Inside%20and%20Outside.md)_
##### CLI commands
_Ver: [NAT  - CLI commands](NAT%20%20-%20CLI%20commands.md)_
