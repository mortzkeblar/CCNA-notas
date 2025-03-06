---
tags:
  - ACL
  - CCNA
date created: Saturday, October 19th 2024, 11:08:14 pm
date modified: Wednesday, November 20th 2024, 11:33:50 pm
---

> Las standard ACL se instalan siempre lo más cerca del destino
> Las extended ACL se instalan siempre lo más cerca del origen

`Es decir, con extended ACL, el router de donde surge el paquete es capaz de controlar este apartado. Asi no es necesario que el paquete vieje por toda la red hasta el destino.`

_Ver: [ACL - Extended, IN OUT](ACL%20-%20Extended,%20IN%20OUT.md)_
_Ver: [ACL - Extended, Examples](ACL%20-%20Extended,%20Examples.md)_

![](Screenshot%20from%202023-12-29%2000-38-49.png)

> #exam 
> Importante: practicar ACL named, es posible encontrar con un ejercicio de este tipo en el examen de CCNA

#### Composition
![](Screenshot%20from%202023-12-29%2000-43-38.png)
- El argumento `IP` engloba a todo el packet stack, sin importar el protocolo L4, los puertos o demás. ![](Screenshot%20from%202023-12-29%2000-46-47.png)

