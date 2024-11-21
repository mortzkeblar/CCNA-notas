---
tags:
  - routing
  - protocol
  - CCNA
date created: Saturday, October 19th 2024, 11:08:14 pm
date modified: Wednesday, November 20th 2024, 11:33:50 pm
---

> _Para más información, puede ver el especificación [RFC 1812](https://datatracker.ietf.org/doc/html/rfc1812#section-5.3.1), punto 5.3.1_

Time-to-live es un campo del IP header (cabezera), esta definido para ser un timer del tiempo de vida de un datagrama. 
- Es un campo de 8-bit y las unidades son segundos

Cada router (o otro dispositivo)  que maneje un paquete debe decrementar el TTL en `-1`, incluso si el tiempo transcurrido sea menor a un segundo. Una vez que el valor de TTL sea igual a 0 (o menor), este debe ser descartado.
- Si el destino no es una dirección multicast, el router debe enviar un mensaje ICMP Time Exceeded, a la fuente del paquete. 

Tambien se puede definir como el limite de saltos en la distancia que un datagrama puede propagar por la red. 


