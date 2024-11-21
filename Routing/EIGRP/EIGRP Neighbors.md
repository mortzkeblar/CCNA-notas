---
tags:
  - CCNA
date created: Saturday, October 19th 2024, 11:08:14 pm
date modified: Wednesday, November 20th 2024, 11:33:50 pm
---
[[EIGRP]], a diferencia de protocolos como [[RIP]] no anuncia toda la [[Routing Table]] en intervalos de tiempo. En cambio, este utiliza el concepto de _neighbor relationships_ para obtener ese resultado. 

Cuando una interface se configura con [[EIGRP]], este comienza a enviar _Hello packets_ para tratar de encontrar vecinos. Lo hace cada 5 segundos usando la multicast address `244.0.0.10`. 
![[Pasted image 20240904025801.png]]

Los routers que ejecutan EIGRP dentro de la misma red, reciben el mensaje _Hello_ y responden al mismo para formar la adyacencia. Esta adyacencia se mantiene a menos que se dejen de recibir una cantidad determinada de mensajes _Hello_, entonces se activa un contador que cuando finalmente expira la ruta es marcada como _unreachable_. 