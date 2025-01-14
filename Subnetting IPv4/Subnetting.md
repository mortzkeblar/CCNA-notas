---
tags:
  - routing
  - CCNA
date created: Saturday, October 19th 2024, 11:08:14 pm
date modified: Wednesday, November 20th 2024, 11:33:50 pm
---
Subnetting hace referencia al proceso de dividir bloques de direcciones dentro de subredes más pequeñas. Se puede difenciar principalmente dos tipos de metodos para la división de bloques. 

- [classful](../Subnetting%20IPv4/classful.md) addressing is IANA/RIRs assigning IP space from Class A, B, or C blocks (_legacy_).
- [classless](../Subnetting%20IPv4/classless.md) ([CIDR](../Subnetting%20IPv4/CIDR.md))  is IANA/RIRs assigning IP space in any size block, as required (_modern standard_). Dentro del proceso de subnetting classless tambien se pueden encontrar diferentes formas para la creación de subredes.
	- [VLSM](VLSM.md) allows any IP subnet within your deployment to be _any_ size (_modern standard_).
	- [[FLSM]] mandates that every IP subnet within your deployment be the _same_ size (_legacy_).


### Attributes of a subnets 
Estas se pueden dividir en 5 atributos principales.
- *Network address* - la primera dirección de la subred, esto implica tener todos los bits de la porción de host en 0s. 
- *Broadcast address* - la ultima dirección de la subred, esto implica tener todos los bits de la porción de host en 1s. 
- *First usable address* - la primera dirección que puede ser asignada a un host, esta se calcula sumando 1B a la network address 
- *Last usable address* - la ultima dirección que puede ser asignada a un host, esta se calcula restando 1B a la broascast address
- *Maximun number of hosts* - la cantidad de direcciones IP disponibles para asignar a hosts. La formula es $2^{x}-2$ siendo $x$ la cantidad de bits en la porción de host (se resta 2 por la network/broadcast address). 



![[Pasted image 20241107024210.png|600]]

### Cheatsheet for subnetting 
![[Pasted image 20250101072118.png]]

#### Subnetting int he /17 - /24 range 

![[Pasted image 20250101134506.png]]


#### Subnetting int he /1 - /16 range 

![[Pasted image 20250101135155.png]]