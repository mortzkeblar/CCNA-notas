---
tags:
  - PNAC
  - CCNA
date created: Saturday, October 19th 2024, 11:08:14 pm
date modified: Wednesday, November 20th 2024, 11:33:50 pm
---

EAPOL es una modificación del protocolo EAP (un protocolo bastante usado en redes inalambricas, lo cual tiene sentido teniendo en cuenta la forma de autenticación). 
Esto nos permite tener autenticación centralizada.


![](Screenshot%20from%202024-01-05%2007-55-49.png)

![](Screenshot%20from%202024-01-05%2008-04-28.png)

- El Server Authentication puede ser de tipo RADIUS o de tipo TACACS+ (bastante usado en entornos Cisco)
	- De esto depende el envio cifrado de los datos  en el segmento entre el Authenticator y el Server
		- Si se usa RADIUS solo se cifra la contraseña
		- SI se usa TACACS+ se cifra usuario y contraseña