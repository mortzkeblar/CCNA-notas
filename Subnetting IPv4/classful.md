---
tags:
  - routing
  - CCNA
aliases:
  - IPv4 address classes
---

El direccionamiento [classful]() se hizo en la etapas tempranas de internet, se trata del asignamiento IP en base a octetos.

![](ip-address-classes-1024x424.png)

Las clases de direcciones IPv4 se determinan por los primeros 1 a 4 bits de la dirección.
- Class A address: empieza con $0$
- Class B address: empieza con $10$
- Class C address: empieza con $110$
- Class D address: empieza con $1110$
- Class E address: empieza con $1111$

![[Pasted image 20241026032921.png]]

La razon por la redes disponibles no sean igual a la cantidad de bits en la porción de red (por ejemplo Class A que marca $2^7$) es porque los primeros bits estan fijos (en este caso el primer bit $0$) entonces solo tenemos 7 bits restantes para cambiar y generar diferentes redes. 

![[Pasted image 20241026033216.png]]



Como se puede ver este es un metodo obsoleto, ya que despues se desarrollaron nuevos metodos  de direccionamiento [classless](classless.md) como [CIDR](CIDR.md).  
