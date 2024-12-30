---
tags:
  - CCNA
date created: Monday, October 21st 2024, 1:51:38 pm
date modified: Wednesday, November 20th 2024, 11:33:50 pm
aliases:
  - Ethernet
  - Ethernet Header
---
Los switches realizan el forwarding en base al contenido dentro del header y el trailer.

![[Pasted image 20241023005647.png]]

> A pesar de que en la imagen se encuentra el _Preamble_ y _Start Frame Delimiter_, estos no forman parte del ethernet header.
> 
## Preamble and SFD 
El _Preamble_ y el _Start Frame Delimiter (SFD)_ se envian en cada ethernet frame, permite sincronizar el receiver clock del dispositivo receptor para el frame entrante. 

La razon por la que no se considera parte del ethernet header es porque sus funciones son exclusivamente de L1 (physical layer) ya que se utiliza para que el receptor pueda interpretar las señales eléctricas recibidas y no influyen en la decisión a tomar para un frame determinado. 

## Destination and source 
Como su nombre lo indica, estos campos corresponden al MAC address de origen y la MAC address de destino. 

> La MAC address tiene un tamaño de 6 bytes (48 bits) y se escribe tipicamente como una serie de 12 caracteres en [[hexadecimal]].

## Type/Length
Es un campo de 2 bytes que puede utilizar para especificar el tipo del paquete encapsulado (p. ej. IPv4 o IPv6) o bien para especificar el tamaño del paquete encapsulado (en bytes). En la mayoria de casos indica el tipo de paquete. 
- Valor menores o iguales a 1500, indica el tamaño del paquete encapsulado 
- Valor de 1536 o mayor, indica el tipo del paquete encapsulado 
	- En este caso el campo se llama _EtherType field_
		- EtherType IPv4: 0x0800 (0d2048)
		- EtherType IPv6: 0x86DD (0d34525)

## Frame Check Sequence
Frame Check Sequence (FCS), es un campo de 4 bytes que se usa para detectar si existen datos corruptos en el frame.

Antes de que el dispositivo envie el frame, utiliza un algoritmo para calcular un _checksum_.
> Un checksum es un pequeño bloque de datos que se añaden al final del frame en el campo FCS 

Cuando el receptor recibe el frame, calcula su propio checksum para el frame y compara si es igual al checksum del emisor.
- Si los checksum son iguales, el receptor puede asegurarse de que el frame no se corrompio en el envio
- Si los checksum son diferentes, se considera que el frame esta corrupto y se descarta 

El algoritmo que se utiliza para calcular el checksum es _Cyclic Redundancy Check (CRC)_



