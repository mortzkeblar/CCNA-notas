---
tags:
  - CCNA
---
Los dispositivos Cisco IOS tienen varios contadores que indican diversos tipos de errores que pueden producirse en la transmisión de datos. Estos se puede ver con `show interfaces <interface>`

![[Pasted image 20241027212815.png|500]]
- _Runts_ - son frames recibidos que son más pequeños que el tamaño minimo que un frame que son 64 bytes.
	- Cuando se quiere enviar un frame con un tamaño menor a 64 bytes, el dispositivo tiene que añadir un _padding_ (todos los bytes en 0s) al final del mensaje para que este tenga un tamaño de 64 bytes. 
	- Estos errores pueden ser causados por colisiones (vease [[collision domain]])
- _Giants_ - son frames recibidos con un payload mayor al MTU configurada en la interface. 
	- Esto se debe generalmente a una mala configuración en la que los dispositivos no tienen valores MTUs consistentes entre sí
- _Input errors_ - es un contador que incluye todos los errores para los frames recibidos 
- _CRC (Cyclic Redundancy Check)_ - es un contador para los frames que fallaron la verificación FCS (Frame Check Secuence) del [[Ethernet Header-Trailer]]. 
	- Esto puede ser causado por problemas electromagneticos, causando corrupción de datos 
- _Output errors_ - es un contador que incluye todos los errores para los frames transmitidos 
- _Collisions_ - es un contador para todos las colisiones detectadas cuando el dispositivo transmite frames. 
	- Para un dispositivo conectado a un hub, se espera colisiones. En un switch, este contador deberia permanecer en 0. 
- _Late collision_ - es un contador que indica los colisiones que sucedieron desde de que el 64 bytes fueron transmitidos 
	- Según CSMA/CD (vease [[collision domain]]), la colisión deberia ocurrir en la transmisión de los primeros 64 bytes. 
		- Cuando ocurre una colisión despues de la transmisión de 64 bytes, puede ser indicativo de que algun dispositivo no este usando CSMA/CD para la verificación de colisiones (probablemente debido a _duplex mismatch_)


## Speed mismatches 
Sucede cuando dos interfaces conectadas intentan comunicarse a diferentes velocidades. El resultado de un speed mismatch es que ambas interfaces tienen un estado down (status) / down (protocol) y no se encuentran operativas.  

![[Pasted image 20241027215253.png]]

## Duplex mismatches 
Sucede cuando dos interfaces conectadas intentan comunicarse con diferentes configuraciones duplex. 
> Aun con un problema de duplex mismatch, las interfaces pueden encontrarse operativas y en estado up/up con transmisión de trafico. 

El principal efecto de un duplex mismatch es que hay una perdida de rendimiento (ver [[Autonegotiation]]). 
- El dispositivo half duplex espera que el otro dispositivo deje de transmitir para que el puede enviar sus datos. Si el dispositivo full duplex transmite datos al mismo tiempo que el half duplex, este ultimo va a interpretar que se produjo una colisión (aunque en realidad no ocurrio).

En estos casos se espera que puedan aumentar los contadores _collisiones_ y _late collisions_ al usar `show interfaces` en el dispositivo half duplex. 

![[Pasted image 20241027220842.png]]


