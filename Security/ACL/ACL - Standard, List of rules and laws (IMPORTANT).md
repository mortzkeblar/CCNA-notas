---
tags:
  - ACL
  - ACL_standard
  - concept
---


- Las ACL Standard son aquellas que van numeradas entre el `1 y 99` y `1300 a 1999`. 
	- Un ACL con un número entre `100 y 199` o `2000 a 2699` se considera `ACL Extended`.
- Las ACL Standard solamente pueden bloquear tráfico basándose en la dirección `IP de origen`. Las `ACL Extended` miran la `IP de origen, IP de destino, puertos de origen y destino`.
- **Por norma general las `ACL Standard` se configurar lo `más cerca posible del destino` y las `Extended ACL lo más cerca del origen`.**
- Se puede escribir `host` para reemplazar una `IP + wildcard 0.0.0.0`
	- `access-list 2 deny 192.168.34.67 0.0.0.0` es equivalente a `access-list 2 deny host 192.168.34.67`.
- Se puede escribir `any` para reemplazar `IP + Wildcard 255.255.255.255`
	- `access-list deny 192.168.34.67 255.255.255.255` es equivalente a `access-list 2 deny host any`
- **Todas las ACL tienen una regla implícita al final denominada `deny any` (Standard ACL) o `deny ip any any` (Extended ACL).**
- Las ACL se ejecutan en orden de manera procedural. Es decir, el router recorre una a una las sentencias y ejecuta la primera con la que haya un match. Luego descarta las sucesivas. 