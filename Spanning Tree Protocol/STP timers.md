---
tags:
  - CCNA
  - SpanningTreeProtocol
date created: Thursday, November 14th 2024, 3:06:07 am
date modified: Wednesday, November 20th 2024, 11:33:50 pm
---
![[Pasted image 20241114022110.png]]
- **Hello** timer - define la frecuencia con la que los BPDUs son enviados (por defecto se envian cada 2 segundos).  
- **Forward delay** timer - define la tiempo de duración de los _listening_ y _learning_ states (por defecto son 15 segundos para cada uno) 
	- Esto significa que un nuevo puerto demora 30 segundos hasta que se le permita reenviar frames (excepto BPDUs)
		- Esta demora sirve para que asegurarse de que no se generen loops en la red, ya que si surge uno es cuestión de segundos para que pueda bajar toda la red. 
- **Max age** timer - determina el tiempo que espera un [[switch]] para cambiar la STP topologia cuando deja de recibir BPDUs por un puerto (por defecto se establece en 20 segundos)
	- Si tenemos el _hello_ timer por defecto, significa que un puerto si no recibe 10 BPDUs el switch decide recalcular la STP topology 

> Los timers del root bridge son usados por todos los switches en la [[LAN]]

![[Pasted image 20241114023803.png]]
En la siguiente imagen se tiene una situación en que un enlace queda inhabilitado. SW1 deja de recibir BPDUs desde SW3 (espera de 20 segundos), luego SW1 decide realizar el calculo de la nueva topologia. Por lo que G0/0 entre en listening state (15 segundos) y luego pasa a learning state (15 segundos), para finalmente pasar a forwarding state. Esta operatoria duro un total de 50 segundos. 