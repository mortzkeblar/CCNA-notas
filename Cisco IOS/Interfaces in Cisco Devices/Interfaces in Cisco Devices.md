---
tags:
  - CCNA
date created: Sunday, October 27th 2024, 4:32:49 am
date modified: Wednesday, November 20th 2024, 11:33:50 pm
---
![[Pasted image 20241027043409.png]]

> El termino _port_ y _interface_ aunque no existe un convenio sobre el uso adecuado, en general cuando se habla de  _port_ se suele hacer referencia a la capa 2 o bien a al puerto fisico del dispositivo. Cuando se habla de _interface_ se suele hacer referencia a capa 3 o bien a las interfaces logicas de un dispositivo.

### Interface speed 
La velocidad de la interface es la taza maxima a la que puede enviar/recibir trafico, se calcula en bits por segundo. Hay interfaces que soportan multiples velocidades.
- FastEthernet soporta velocidades de 10 y 100 Mbps, por la cual es llamada _10/100 interfaces_
- GigabitEthernet soporta velocidades de 10, 1000 y 1000 Mbps (1 Gbps), por la cual es llamada _10/100/1000 interfaces_

La velocidad a la que operan las interfaces puede ser seteada a través de `speed { speed | auto }` siendo `auto` la opción por defecto. 

![[Pasted image 20241027191024.png|400]]


### Interface duplex 
Esta hace referencia a que si la interface es capaz de enviar y recibir trafico al mismo tiempo. Se pueden encontrar dos tipos. 
- _Half Duplex_, la interface puede enviar y recibir datos pero no al mismo tiempo 
- _Full Duplex_, la interface puede enviar y recibir datos al mismo tiempo 

Un ejemplo half duplex son las redes wi-fi (IEEE 802.11). Los dispositivos que se conectan a la red wi-fi comparten el mismo medio fisico (radio frecuencia) estos deben esperar su turno para comunicarse, no pueden enviar y recibir datos al mismo tiempo. 

> Otro ejemplo half duplex es el del hub, donde los hosts se encuentran en el llamada _medio compartido_ por lo cual es propenso a generar colisiones de frames en un [[collision domain]]. Para evitar eso, se utiliza una configuración half duplex. 

Un ejemplo de full duplex es una conexión cableada ethernet LAN usando un [[switch]], el cual permite el envio/recepción de datos al mismo tiempo.

Para configurar una interface duplex se puede usar `duplex { auto | full | half }` siendo `auto` la opción por defecto. 

![[Pasted image 20241027202443.png|500]]

