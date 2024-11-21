---
tags:
  - routing
  - CCNA
date created: Saturday, October 19th 2024, 11:08:14 pm
date modified: Wednesday, November 20th 2024, 11:33:50 pm
---

Classless inter-domain routing es un metodo de asignamiento de IPs públicas, introducido en el '93 por la IETF con los siguientes objetivos.
- Tratar de resolver el problema de agotamiento de direcciones IPv4.
- Relantizar el crecimiento de la tablas de enrutamiento en los routers de internet.

Ademas se trato de solventar el problema de desperdicio de direcciones IP con los metodos [classful](classful.md) de direccionamiento. 

CIDR eliminó los requisitos fijos de `/8`, `/16` y `/24` para cada clase y permitió dividirla en todo el rango unicast (primer octeto 0, 225), lo que ahora llamamos [[Subnetting]]. 

Para poder simplificar la notación, se adopto la _slash notation_. P. ej. la subnet `255.255.255.128` era equivalente a `/25`.

> Este tipo de notación tambien es conocido como notación CIDR, anteriormente a la introducción de CIDR el tamaño del prefijo se indicaba con la mascara de red. 

![600](2018-06-08_11-57-24-b43b39665963f818d49609459d652f04.png)

**División de un bloque /24 en varias subredes**
![[Pasted image 20241107014750.png|500]]

## /31 and /32 prefix lengths 
- Un /31, tiene un unico bit para host. Si usamos la formula $2^{x}-2$ nos devuelve un resultado 0. Un solo bit de host solo permite dos direcciones, las cuales son tomadas por la network/broadcast address respectivamente, pero queda sin especio para direcciones de host.
	- Es comun ver esta subnet en conexiones point-to-point entre dos routers, en cada extremo se asigna una IP y se omiten la network/broadcast address. 

![[Pasted image 20241107032959.png|500]]

- Un /32 subnet mask, se usa para especificar una unica direccion IP en una ruta. En general es raro encontrar esta configuración en una interface (a excepción de cosas como [[OSPF]]). 