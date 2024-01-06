> NAT, VLSM y CIDR son tres técnicas ampliamente utilizadas para prolongar el uso de direcciones IPv4 en internet.
> 
> Sin embargo NAT trajo limitaciones como la conexión end-to-end, etc.

### Network Address Translation (NAT) - Port Address Translation (PAT)

##### RFC 1918
Bloque de IP privadas:
```
- 10.0.0.0/8 (10.0.0.0 - 10.255.255.255)
- 172.16.0.0/12 (172.16.0.0 - 172.31.255.255)
- 192.168.0.0/16 (192.168.0.0 - 192.168.255.255)
```

> Problemas: no respetar el bloque de direcciones IP privadas puede generar problemas.
> Si una organización configura una LAN por ej, asignándoles IPs 90.x.x.x no va a tener problemas (a pesar de ser IPs publicas están contenidas dentro de la LAN). El problema surge que si algún host de esa LAN trata de conectarse a alguna dirección IP que este (por ejemplo) en 90.100.2.2 (que se entiende es una dirección publica en internet) y justamente existe una IP privada en esa LAN con la misma dirección. La red lo va a redirigir hacia esa IP privada y no a la IP publica en internet. 
> 
> Para más detalle ver [aqui](https://youtu.be/wwCaEkwu0y0?list=PL2A7l6PiV52esSwosIAO86zf0RGe2pjTZ).


### Tipos de NAT
- `Dynamic NAT`
- `Static NAT`
- `Overloaded NAT/PAT`
- `Dynamic NAT with overload`
- `NAT-T (NAT Traversal)`
- `Source NAT`
- `NAT64`


> Importante: en el examen de certificación es comun ver algun ejercicio relacionada a un mal funcionamiento de NAT. Por general suele ser con `inside/outside` mal configurado. Tomar enfasis en ese punto.


#### Formalidades en la syntaxis
- Red interna LAN (`inside`)
- Red externa WAN (`outside`)
### Tipos de NAT
##### Dynamic NAT
_Ver: [NAT - Dynamic](NAT%20-%20Dynamic.md)_
##### Static NAT
_Ver: [NAT - Static](NAT%20-%20Static.md)_
##### Overload NAT (PAT)
_Ver: [NAT - Overload or PAT](NAT%20-%20Overload%20or%20PAT.md)_
##### Overload Dynamic
_Ver: [NAT - Overload Dynamic (Multiple IP address)](NAT%20-%20Overload%20Dynamic%20(Multiple%20IP%20address).md)_

### Misc
##### Inside/Outside NAT examples
_Ver: [NAT - Inside and Outside](NAT%20-%20Inside%20and%20Outside.md)_
##### NAT - CLI commands
_Ver: [NAT  - CLI commands](NAT%20%20-%20CLI%20commands.md)_
