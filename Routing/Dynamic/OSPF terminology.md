#exam En el examen de certificación se espera que tener conocimientos para configurar y hacer troubleshooting de [OSPF](OSPF.md) en IPv4 e IPv6, ambos en single y multi-area.


Algunos terminologias usadas en OSPF son:

-  [cost](cost.md) 
- _Area ID_ - un area OSPF es un grupo de routers dividido dentro de subdominios basados en ID de area, cada router dentro del mismo area comparte el mismo _link state_ información. Es identificable por un ID de area de 32-bit, se pueden representar en decimales o decimales con puntos (como las [IP Address](../../NetWarriors/IP%20Address.md)). El area 0 y area 0.0.0.0 significan lo mismo. 
- _Link_ - un enlace es una conexión hacia otro router, la _OSPF topology table_ se denomina _Link State Database_.
- _Link state_ - es el estado del enlace con otro router, el _link state database_ consiste de una lista de el estado de los enlaces entre los routers en la misma área. 
- _Link State Database (LSBD)_ - es una lista de todos los estados de enlace de otros routers en al red. LSBD es esencialmente la topologia de la red. La base de datos se construye a partir del intercambio de LSAs. 
- _Link State Advertisement (LSA)_ - son paquetes de datos [OSPF](OSPF.md) que contienen la información de enrutamiento y el estado de los enlaces que es compartido entre los routers [OSPF](OSPF.md). 
- _Process ID_ - es designado por el comando `router ospf [1-65535]`, el process ID es necesario para identificar una unica instanciade un OSPF database. El process ID no necesita hacer match con sus vecinos (no es igual que el ASN de EIGRP). 
- Router ID (RID) #TODO 
- _Network type_ - 