---
tags:
  - DHCP
  - CCNA
---



_Ver: [DHCP](../DHCP.md) y [DHCP - Discover Examples](DHCP%20-%20Discover%20Examples.md)_

Este tipo de ataque consiste basicamente en falsificar que somos un servidor DHCP. Cuando otros dispositivos se conecten se van a reenviar a la red que podemas generar (ej: 10.0.0.0/24) con un gateway que definamos (ej: 10.0.0.1, nuestra propia IP). 
Esto genera que el trafico de la red se reenvie al falso gateway y asi interceptar todo el trafico de la red LAN, las victimas no se darian cuanta puesto que podemos reenviar el trafico hacia/desde internet para que sus solicitudades salgan sin problemas. 

Cuando alguien lanza una broadcast para poder conectarse a la red, lanzamos un ataque de tipo _DHCP starvation attack_, esto consiste en que el atacante envia miles de solicitudes _discover_ y _request_ al servidor saturando el pool de direcciones IP asignadas.
Cuando un cliente quiera conectarse a la red, el servidor le devolvera _NACK_ por falta de direcciones IP, entonces aquí es donde entra el DHCP server falso (Rogue DHCP) que es al que finalmente termina conectándose el cliente.

![](Screenshot%20from%202024-01-05%2008-54-38.png)

### Mitigación - DHCP Snooping
_Ver: [DHCP Snooping - Mitigation](DHCP%20Snooping%20-%20Mitigation.md)_

### Ejemplos de DHCP Snooping
_Ver: [DHCP Snooping - Example](DHCP%20Snooping%20-%20Example.md)_
