---
tags:
  - CCNA
date created: Saturday, October 19th 2024, 11:08:14 pm
date modified: Wednesday, November 20th 2024, 11:33:50 pm
---
Un dominio de colisión es un segmento de red en el que la transmisión simultanea provoca coalisiones, esto era comun en los hubs. Lo que hacia que los dispositivos conectados tuvieran que funcionar en half duplex. 

![[Pasted image 20241027200841.png|400]]

A diferencia de un hub, en un [[switch]] cada host se encuentra en su propio dominio de colisión ya que los switches pueden almacenar los frames en memory y hacer forwarding de ellos. Lo cual permite que los hosts puedan operar en full duplex. 

![[Pasted image 20241027201418.png|500]]

### Carrier-sense multiple access with collision detection 
CSMA/CD es un metodo para facilitar la comunicación en un medio compartido (una [[LAN]] que usa un hub). 
- Carrier-sense significa que los dispositivos deben detectar cuando el medio esta en uso antes de transmitir el mensaje 
- Multiple access significa que se usa un medio compartido 
- Collision detection significa que si ocurre una colisión, los dispositivos conectados a ese medio deben detectarlo 

![[Pasted image 20241027203922.png]]