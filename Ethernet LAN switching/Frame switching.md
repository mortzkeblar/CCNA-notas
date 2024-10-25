---
tags:
  - CCNA
---
Este apartado cubre como los [[switch]]es usan el campo de origen y destino del [[Ethernet Header-Trailer]] para construir las _[[MAC address table]]_ y poder reenviar los frames a sus destinos dentro de la [[LAN]]. 

## MAC address learning
Cuando un [[switch]] recibe un frame, este examina la MAC address de origen y crea una entrada en la MAC address table asociandolo al puerto de donde recibio el frame. 

![[Pasted image 20241023022534.png]]
### Dynamic MAC addresses 
El proceso descrito anteriormente para el aprendizaje de MAC address, se denomina _dynamic MAC address_. Ya que estos se aprenden automáticamente así como se elimina luego de cinco minutos, si es que el switch no recibió un frame desde esa MAC address. Por contraparte también se pueden definir MAC address estáticas. 

## Frame flooding and forwarding 
El switch realiza el forwarding mediante usando la información contenida de la [[MAC address table]]. 
- Los frames enviados a un unico host de destino se llama _unicast frame_
- Los frames enviados a un unico host de destino que ya tiene una entrada en la MAC address table se llaman _known unicast frame (forward)_

Cuando el switch recibe un frame para el cual no tiene una entrada que corresponda a la MAC address de destino. Entonces el switch realiza un _flooding_ del frame en la red, reenvia este a todos los puertos de salida excepto al puerto de origen del frame. A la espera de que el receptor reciba la respuesta y generar una entrada para este cuando envie la respuesta de retorno. 

![[Pasted image 20241023024128.png|500]]
- Los frames enviados a un unico host de destino que no tiene una entrada en la MAC address table se llaman _unknown unicast frame (flood)_

![[Pasted image 20241023024512.png|500]]

> Un switch es transparente al conectar hosts, se puede entender como que host A y host B estan conectados solo por un cable sin nada entre medio. Por eso cuando un mensaje pasa por el switch no se considera un _hop_. Además, los switches NO modifican los frames que reciben, solo hacen el reenvio o flooding dependiendo la situación. 


