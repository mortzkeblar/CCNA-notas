---
tags:
  - CCNA
date created: Monday, November 18th 2024, 3:17:11 am
date modified: Wednesday, November 20th 2024, 11:33:50 pm
---
_Dynamic EtherChannel_ se apoya en protocolos de negociación, el switch usa el protocolo para enviar mensajes entre ellos y hacer el proceso de negociación par formar finalmente el etherchannel link. 

> Al tratar de configurar etherchannel se pueden producir layer 2 loops. Por lo que se recomienda usar mayormente dynamic etherchannel sobre static. 

Cisco [[switch]]es soportan dos protocolos de negociación dynamic etherchannel: 
- _Port Aggregation Protocol (PAgP)_ - propietario de Cisco 
- _Link Aggregation Control Protocol (LACP)_ - definido en el IEEE 802.3ad

**Etherchannel mode combinations**
![[Pasted image 20241118032436.png]]
## PAgP configuration
Cisco PAgP permite configurar hasta 8 enlaces fisicos como un unico enlace [[EtherChannel]]. 

Para configurar un enlace PAgP se usa `channel-group <group-number> mode {disarable | auto}` dentro de interface config.
- `group-number` identifica el etherchannel en el [[switch]], permite saber que puertos pertenecen a determinado etherchannel 
Los modos `disarable` y `auto` son los que determinan si se forma o no el enlace etherchannel.
- `disarable` mode - intentara de forma activa formar un enlace PAgP enviando mensajes a su vecino 
- `auto` mode - no busca de forma activa formar un enlace PAgP pero respondera los mensajes PAgP que pueda recibir de un vecino y formara el etherchannel
	- Si ambos extremos se encuentra en `auto`, no se formara el etherchannel 

![[Pasted image 20241118000856.png]]

Para comprobar el estado en el que se encuentran los etherchannel en el [[switch]] se usa `show etherchannel summary` (para PAgP tambien se puede usar `show pagp neighbor` que muestra información sobre los puertos PAgP del switch vecino).

![[Pasted image 20241118000952.png]]
Se puede observar que existen varias flags con su respectivo significado.
- El `S` flag indica que opera como un puerto de capa 2, para que pueda operar como una interface de router de capa 3 se puede usar `no switchport` en la interface. 
- La `I` flag indica que el port-channel no logro establecer una negociación con su vecino, por lo cual los puertos funcionan en modo _stand-alone_ o independientes. 
- Una vez que el etherchannel se forma, esta toma las flags `SU` y los puertos el flag `P`

> Las configuraciones hechas en el port-channel son heredadas por los puertos miembros, al igual que si se realizan de forma inversa, estas son heredadas por el port-channel. La recomendación es que las configuraciones se hagan directamente sobre la interface port-channel.

## Link Aggregation Control Protocol (LACP) configuration
_Link Aggregation Control Protocol_ o LACP (IEEE 802.3ad), permite configurar hasta 16 enlaces fisicos como un unico enlace [[EtherChannel]], aunque solo puede haber un maximo de 8 enlaces activos y los restantes permanecen como failover. 

![[Pasted image 20241118031923.png]]

La principal diferencia a la hora de configurar LACP, es que los _keywords_ usados en este caso son `active` y `passive` (en vez de `disarable` y `auto` en PAgP). 
- `active` mode - es equivalente a `desirable` mode en PAgP, busca de forma activa establecer un etherchannel con su vecino mediante el envio de mensajes LACP 
- `passive` mode - es equivalente a `auto` mode en PAgP, no busca formar etherchannel de forma activa pero si respondera a los mensajes LACP que recibe y formara el etherchannel 

![[Pasted image 20241118030748.png]]
Para configurar un enlace LACP se usa `channel-group <group-number> mode {active | passive}`

![[Pasted image 20241118031351.png]]
Para comprobar el estado de los enlaces LACP se puede usar tanto `show etherchannel summary` como tambien `show lacp neighbor` que muestra información sobre el estado del puerto vecino. 
