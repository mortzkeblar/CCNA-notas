---
tags:
  - CCNA
date created: Tuesday, October 29th 2024, 4:08:47 am
date modified: Wednesday, November 20th 2024, 11:33:50 pm
---
Es necesario recordar que siempre que un host envia un dato, el paquete de capa 3 siempre sale encapsulado dentro de un frame.

La MAC address de destino depende del destino del paquete IP. 
- Si el paquete se envia a otro host dentro de la misma red, la MAC address es el propio host final. Por lo que no necesario pasar por un router. 

![[Pasted image 20241029041224.png|500]]
- Si el paquete se envia a otro host fuera de su red local, este debe enviar el paquete a su _[[Default gateway]]_ que es el router que le proporciona conectividad con otras redes. 
	- Es importante ver que en este caso, la MAC address de destino que se envia desde el PC1 es el del router que actua como [[Default gateway]]. 

![[Pasted image 20241029041549.png]]