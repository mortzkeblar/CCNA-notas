---
tags:
  - routing
  - dynamic
  - OSPF
  - CCNA
date created: Saturday, October 19th 2024, 11:08:14 pm
date modified: Wednesday, November 20th 2024, 11:33:50 pm
---

#exam En el examen de certificación se espera que tener conocimientos para configurar y hacer troubleshooting de [OSPF](OSPF.md) en IPv4 e IPv6, ambos en single y multi-area.


Algunos terminologias usadas en OSPF son:

-  [OSPF cost](OSPF%20cost.md) 
- _Area ID_ - un area OSPF es un grupo de routers dividido dentro de subdominios basados en ID de area, cada router dentro del mismo area comparte el mismo _link state_ información. Es identificable por un ID de area de 32-bit, se pueden representar en decimales o decimales con puntos (como las [IP](../../labs/NetWarriors/IP.md)). El area 0 y area 0.0.0.0 significan lo mismo. 
- _Link_ - un enlace es una conexión hacia otro router, la _OSPF topology table_ se denomina _Link State Database_.
- _Link state_ - es el estado del enlace con otro router, el _link state database_ consiste de una lista de el estado de los enlaces entre los routers en la misma área. 
- _Link State Database (LSBD)_ - es una lista de todos los estados de enlace de otros routers en al red. LSBD es esencialmente la topologia de la red. La base de datos se construye a partir del intercambio de LSAs. 
- _Link State Advertisement (LSA)_ - son paquetes de datos [OSPF](OSPF.md) que contienen la información de enrutamiento y el estado de los enlaces que es compartido entre los routers [OSPF](OSPF.md). 
- _Process ID_ - es designado por el comando `router ospf [1-65535]`, el process ID es necesario para identificar una unica instanciade un OSPF database. El process ID no necesita hacer match con sus vecinos (no es igual que el ASN de EIGRP). 
- [RID](RID.md)  (RID) 
- _Network type_ - es el tipo de red conectada en la interface [OSPF](OSPF.md). 
- _Neighbors_ - dos routers que tiene interfaces en una red comun se consideran neighbors. Los neighbors se descubren y mantienen mediante el procolo _Hello_. 
- _Designated router (DR)_ - este es el punto central para el intercambio de información de enrutamiento en la red broadcast. Se base en la reducción del trafico que pasa por un interface compartida (p. ej. una interface ethernet con cinco routers). El DR se elige a través de los paquetes  _Hello_ que atraviesan el área. Si todos los routers tiene la misma prioridad, se elige al router con el RID más alto (normalmente la [IP](../../labs/NetWarriors/IP.md) más alta). Tambien se designa un Backup Designated Router (BDR) en caso de que el DR fallé.
- _Internal router_ - un router con todas las redes conectadas directamente que pertenecen al mismo area (no tiene porque ser un área 0).
- _Area border router (ABR)_ - un router con redes en más de una área.
- _Backbone router_ - un router con una interface en el backbone (área 0).
- _AS boundary router (ASBR)_ - un router que intercambia actualizaciones con routers en otras AS ([AS](AS.md)). 


Las entradas de la tabla de enrutamiento [OSPF](OSPF.md) pueden ser internas a esa área, representados por una "O" en la tabla. O desde un ABR, representados por una "O IA". Un tercer tipo de entrada "O E2" representa rutas desde un ASBR. 