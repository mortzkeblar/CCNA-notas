---
tags:
  - CCNA
date created: Sunday, October 27th 2024, 8:53:47 pm
date modified: Wednesday, November 20th 2024, 11:33:50 pm
---

Tanto la velocidad de transferencia como el duplex de una interface puede usar la autonegotiation para una configuración automatica. En este proceso, cada dispositivo anuncia a su vecino sus capacidades y estos se ponen de acuerdo para usar el mejor modo disponible de acuerdo a la prioridad. 

![[Pasted image 20241027204159.png]]
![[Pasted image 20241027204241.png|500]]

En caso de que en un extremo este habilitado la autonegotiation pero en el otro se haya asignados valores manuales, el dispositivo auto toma las siguientes casos.
- _Speed_ - trata de detectar la velocidad a la que funciona su vecino, si no tiene exito se configura con la velocidad más baja soportada(p. ej. 10 Mbps en una interface 10/100/1000)
- _Duplex_ - Si la velocidad es 10 o 100 Mbps, usa half duplex. Si la velocidad es mayor o igual a 1000 Mbps, usa full duplex. 


