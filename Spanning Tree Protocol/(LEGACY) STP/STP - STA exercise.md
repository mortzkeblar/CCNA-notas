---
tags:
  - STP
  - CCNA
date created: Saturday, October 19th 2024, 11:08:14 pm
date modified: Wednesday, November 20th 2024, 11:33:50 pm
---
_Ver: [STP - STA](STP%20-%20STA.md) _
![normal](../../_anexos_/Screenshot%20from%202024-01-02%2012-49-57.png)

## Solución

![](../../_anexos_/Screenshot%20from%202024-01-04%2011-02-53.png)
- El Root Bridge (RB), si bien todos tienen el mismo valor en la _prioridad + VLAN ID_, es el valor de MAC address el que decide el RB, en este caso es S1 por tener el valor más bajo
- Ahora toca decidir por los Root Ports (RP) 
	- S1 automaticamente pone todos sus puertos en _designated_ y comienza a enviar BPDUs a las demas switches
	- S2 tiene dos rutas para llegar al RB, este elige la ruta directa
		- Ruta directa, costo 4 
		- Ruta indirecta (S3, S4, S2), costo 12
	- S3, dos rutas. La mejor opción sigue siendo la ruta directa.
		- Ruta directa, costo 4
		- Ruta indirecta(S2, S4, S3), costo 12
	- S4, dos rutas. En este caso  es el BID más bajo el que genera el desempate
		- Ruta a través de S2
		- Ruta a través de S3
	- S5, dos rutas. 
		- Ambas provienen de S4 pero por diferentes interfaces, en este caso la decisión se toma en la interfaz de mejor númeración.
			- Ruta a través de fa0/19
			- Ruta a través de fa0/23