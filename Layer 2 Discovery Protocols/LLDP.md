---
tags:
  - CCNA
---
**Link Layer Discovery Protocol (LLDP)** es protocolo de descubrimiento de vecinos, este es la versión vendor-neutral desarrollado por el IEEE (**IEEE 802.1ab**) equivalente a [[CDP]].

> Los mensajes LLDP son enviados a la multicast MAC address `0180.c200.000e`. Al igual que en CDP, cuando un [[switch]] recibe un mensaje LDDP no realiza el flooding sino que se lo conserva para si mismo.

![[Pasted image 20250114063400.png|400]]

### Configuring and verifying LLDP 
En los dispositivos Cisco, LLDP no esta habilitado por defecto. Esto se puede hacer con `lldp run`, para verificar se puede usar `show lldp`.

![[Pasted image 20250114063601.png]]
- `LLDP advertisement` - cada cuanto se envia un mensaje LLDP, por dfecto en 30 segundos
	- Puede ser modificado con `lldp timer <seconds>`
- `LLDP hold time` - valor por defecto de 120 segundos, equivalente a `holdtime` de [[CDP]] 
	- Puede ser modicado con `lldp holdtime <seconds>`
- `LLDP reinitialization delay` - valor de 2 segundos por defecto, agrega un delay en situaciones donde sucede _bouncing_ y al alternación del estado del puerto en UP / DOWN, esto da tiempo a que la interface puede estabilizarse antes de volver a enviar nuevamente menajes LLDP
	- Puede ser modificado con `lldp reinit <seconds>`

Una de las mayores diferencias con [[CDP]], es que LLDP te permite controlar el envio o la recepción de mensajes por interface de forma independiente. Esto se puede lograr con `[no] lldp transmit` y `[no] lldp receive` en el _interface config mode_ respectivamente, para verificar esta configuración se puede usar `show lldp interface <interface>`

![[Pasted image 20250114065353.png]]

Para esta la posibilidad de ver las estadisticas sobre el trafico LLDP con `show lldp traffic`

![[Pasted image 20250114073454.png]]

### Viewing LLDP neighbors 
Para revisar la table de vecinos LLDP se usa `show lldp neighbors`. 

![[Pasted image 20250114073546.png]]
Para tener más detalles se puede usar `show lldp neighbors detail`, en caso de especificar una dispositivo se debe usar `show lldp entry <device>`

**LLDP `show` commands**

![[Pasted image 20250114073817.png]]

**LLDP configuration commands**

![[Pasted image 20250114073835.png]]


