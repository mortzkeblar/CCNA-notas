---
tags:
  - CCNA
date created: Saturday, October 19th 2024, 11:08:14 pm
date modified: Wednesday, November 20th 2024, 11:33:50 pm
---
*Variable Length Subnet Masks* (VLSM) es un metodo [classless](classless.md) que surgio como una evolución de [[FLSM]]. 

El concepto básico de VLSM es muy simple: se toma una red y se divide en subredes fijas, luego se toma una de esas [subredes](https://es.wikipedia.org/wiki/Subred "Subred") y se vuelve a dividir, tomando bits "prestados" de la porción de hosts, ajustándose a la cantidad de hosts requeridos por cada segmento de nuestra red.

Algunos de los protocolos que soportan [VLSM]() son [RIPv2](../Routing/RIP/RIPv2.md) , [[OSPF]] y las versiones más recientes de BGP y [[EIGRP]].


### VLSM scenario
![[Pasted image 20241108004042.png]]

Se nos asigna un bloque de direcciones /24 para realizar las asignaciones segun el diagrama. El proceso a alto nivel de VLSM consiste en:
- Asignar la subnet más grande al principio del bloque de direcciones 
- Asignar la segunda subnet más grande despues de esa 
- Repetir el proceso para todas las subnets 

Segun el diagrama el orden de asignación seria: Toronto LAN A, Tokyo LAN A, Toronto LAN B, Tokyo LAN B y la conexión WAN. 

![[Pasted image 20241108004731.png|600]]

#### Assigning Toronto LAN A subnet 
Esta requiere de 122 direcciones de host. Por lo cual necesitamos tomar un bit de la porción de red, quedarian 7 bits de la porción de host que calculado son $2^{7}-2=126$ hosts. Esto genera la red 10.89.100.0/25.

![[Pasted image 20241108004754.png|500]]
#### Assigning Tokyo LAN A subnet 
Despues de la primera asignación mas grande, sigue Tokyo LAN A subnet. Esta se debe asignar a partir de la red restante que quedo al dividir al red en /25 para Toronto LAN A. Es decir debemos usar la red 10.89.100.128/25. 

Esta red requiere de 62 hosts, por lo cual se puede asignar una red /26 ya que esta puede contener $2^{6}-2=64$ hosts. Esto genera la red 10.89.100.128/26.

![[Pasted image 20241108030040.png|500]]

#### Assigning Toronto LAN B subnet 
Continuamos con el mismo procedimiento ya realizado anteriormente pero esta vez trabajando sobre la red sobrante que seria 10.89.100.192/26.

En este caso necesitamos 30 hosts por lo cual una red /27 encaja perfectamente ya que nos permite tener $2^{5}-2=30$ hosts. Esto forma la red 10.89.100.192/27

![[Pasted image 20241108030817.png|500]]

#### Assigning Tokyo LAN B subnet 
En este caso usamos la red que queda libre, 10.89.100.224/27.

Los requisitos para esta red es de 11 hosts. Por lo cual podemos usar un bloque /28 que nos permite tener $2^{4}-2=14$ hosts. Esto forma la red 10.89.100.224/28.

![[Pasted image 20241108031523.png|500]]

#### Assigning the WAN connection subnet 

Se trata de una conexión P2P, por lo cual solo necesitamos 2 direcciones para los hosts. Tenemos como sobrante de la operación anterior la red 10.89.100.240/28.

En este caso podemos usar tanto un bloque /30 como /31, para el ejemplo se usa un bloque /30. Por lo cual la red generada es 10.89.100.240/30 que permite tener 2 hosts más la dirección de red y la dirección broadcast. 

![[Pasted image 20241108031920.png|500]]

Luego de realizado todas las subredes solicitadas, aun quedan bloques disponibles desde la IP 10.89.100.244 a 10.89.100.255. Estas son libres de ser usadas cuando sean necesarias. 