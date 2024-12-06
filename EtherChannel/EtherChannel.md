---
tags:
  - CCNA
date created: Sunday, November 17th 2024, 3:17:58 am
date modified: Wednesday, November 20th 2024, 11:33:50 pm
---
> ***Bandwidth*** es el número total de bits que pueden ser transferidos sobre una conexión por segundo (4x enlaces de un 1Gbps significa 4Gbps de bandwidth). Es importante diferenciar el concepto de bandwidth del de **_speed_**, esta ultima hace referencia al ratio maximo de transferencia de una comunicación (o conexión) en particular. 

_EtherChannel_ es una tecnologia que permite agrupar varios enlaces fisicos como un único enlace lógico (lo que permite superar la barrera que pueda establecer [[STP (LEGACY)|STP (LEGACY)]] para prevenir los layer 2 loops), esto permite mayor disponibilidad del bandwidth además de generar resiliencia y facilidad en la administración. 

![[Pasted image 20241117105419.png]]

> Las interfaces logicas creadas mediante EtherChannel son llamadas _Port Channels_ en Cisco IOS. En el contexto del CCNA, se considera ambos terminos equivalentes. 

Los frames reenviados por el puerto etherchannel, se distribuyen sobre todos los enlaces fisicos, esto es llamado _load balancing_ o _load distribution_.

![[Pasted image 20241117110144.png]]
El puerto al ser tratado como un solo enlace logico, no existen riesgos de que pueda generar un layer 2 loop, como se ve en la imagen anterior. Esto se puede comprobar con `show spanning-tree`.

![[Pasted image 20241117110540.png]]

## EtherChannel configuration 
Se tienen dos caminos para poder configurar etherchannel: 
- *[[Dynamic EtherChannel]]* - este modo hace que los [[switch]]es usen un protocolo para negociar entre ellos la formación de un etherchannel link 
- *[[Static EtherChannel]]* - este modo fuerza al switch a formar un etherchannel con los puertos especificados, sin ningun tipo de negociación con el vecino 

## Physical port configurations 
Al configurar etherchannel, es importante que los puertos de ambos extremos coincidan en las configuraciones, entre ellas:
- Speed  
- Duplex 
- Modo operacional (access o trunk)
	- Si esta en trunk mode, [[VLAN]]s permitidas y [[VLAN Native]] 
	- Si esta en access mode, VLAN de acceso 

## EtherChannel load balancing 
Etherchannel load balancing es una función que determina por donde son reenviados los frames, basado en el flujo o _flow_. 
- Todos los mensajes que pertenecen al mismo flujo sobre reenviados por el mismo enlace dentro del etherchannel 

> Un _flow_ es una comunicación entre dos nodos  

![[Pasted image 20241118033422.png]]
![[Pasted image 20241118044944.png]]

El load balancing de EtherChannel no siempre se realiza de forma equitativa sobre todos los enlaces, sin embargo se puede modificar la logica que utiliza para realizar el load balancing con el comando `port-channel load-balance <parameter>`en el modo global config. SI el comando se ejecuta sin parametros, muestra la configuración actual, por defecto se usa `src-dst-ip`.

**EtherChannel load-balancing parameters**
![[Pasted image 20241118045223.png]]

## Layer 3 EtherChannel
Los [[EtherChannel]] de capa 3 consisten en puertos enrutados (like a router). Al igual que cuando se configura un multilayer switch, se puede hacer uso del comando `no switchport` para que un puerto tenga capacidad de reenviar paquetes. 

> Una pequeña ventaja de los etherchannel de capa 3 frente a los de capa 2 es que no existe riesgo en generar layer 2 loops. 

![[Pasted image 20241118050406.png]]
