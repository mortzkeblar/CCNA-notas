---
tags:
  - NAT
  - example
---
#TODO que es RFC 1918?
# Address allocation for Private Internet

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
