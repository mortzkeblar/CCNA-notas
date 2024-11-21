---
tags:
  - CCNA
date created: Sunday, October 27th 2024, 11:14:03 pm
date modified: Wednesday, November 20th 2024, 11:33:50 pm
---
El cable de cobre (a menudo llamado cable ethernet, aunque el estandar puede usar tanto cables de cobre como de fibra).

![[Pasted image 20241027233523.png|500]]

Existen dos tipos de cables que se usan para estas conexiones.
- _Unshielded Twisted Pair (UTP)_ - este cable no tiene un blindaje metalico alrrededor (el cual se usa para reducir la _interferencia electromagnetica o EMI_)
- _Shielded Twisted Pair_ cable - los ocho alambres de este cable se envuelven en dos pares de 4 alambres cada uno. La torción de los alambres reducen el _EMI_ entre los alambres de cada par. 

## IEEE 802.3 standards (copper)
La IEEE define varios estandares para las conexiones ethernet que abarca velocidades, tipos de cable y distancias. Las cuales se segmentan en nombres.
- Un nombre se deriva de la velocidad de transmisión maxima soportada 
- El nombre del el _IEEE Task Group_ se usa para referirse al propio estandar. 
	- Estos nombres comienzan con IEEE 802.3 seguidos de una letra 
- El tercer nombre indica tanto la velocidad como el tipo de cable 

![[Pasted image 20241027235354.png]]
 
 **Common UTP Cable standards**
 ![[Pasted image 20241028001150.png]]

## Straight-through and crossover cables 
Hoy en dia todos los cables UTP usan el esquema de cuatro pares de hilos (8 hilos), no todos los estandar ethernet siguen el esquema. 
- 10BASE-T usa dos pares (cuatro hilos)
- 100BASE-T usa dos pares (cuatro hilos)
- 1000BASE-T usa cuatro pares (ocho hilos)
- 10GBASE-T usa cuatro pares (ocho hilos)

Cada hilo dentro de un cable esta conectado a uno de los 8 pines del conector 8P8C (tambien registrado como _Jack-45_ o RJ45). Cada par de hilos forma un circuito electrico entre los dos dispositivos conectados.

> En las conexiones 10BASE-T y 100BASE-T es especialmente importante ya que se deben garantizar que los pines de un lado esten en el mismo orden que en el otro lado de la conexión. 

Existen dos tipos de cables, las cuales difieren en donde se conectan los pines de un lado del cable con los pines del otro lado del cable. 
- Straight-through cables (o cables directos)
- Crossover cables (o cables cruzados)

### Straight-through cables 
10BASE-T y 100BASE-T usan dos pares de hilos, uno para cada dirección de la comunicación.
- El par conectado en los pines 1 y 2 
- El par conectado en los pines 3 y 6 

![[Pasted image 20241028003154.png|500]]
Esto funcionan bien cuando se conecta PC-switch, pero falla a la hora de querer conectar un switch-switch, PC-PC o router-router ya que en este caso chocan estos dispositivos ya que usan los mismos pines para la Tx/Rx de datos. Lo cual impide la comunicación entre estos.

![[Pasted image 20241028003659.png|500]]

### Crossover cables 
Un cable cruzado conecta par de pines opuestos. 
- Pin 1 y 2 en un lado se conectan con el Pin 3 y 6 del otro lado, y viceversa.

Esto permite que se puedan usar los mismo par de pines para la transmisión de datos entre dos dispositivos. 

![[Pasted image 20241028004018.png|500]]

**Pares de pines Tx/Rx según el dispositivo**
![[Pasted image 20241028004434.png]]

### Auto MDI-X
_Auto Medium-Dependent Interface Crossover_, esta caracteristica permite cambiar la posición de los pines Rx/Tx dependiendo el dispositivo al que se conecta. 
![[Pasted image 20241028004749.png]]


### 1000BASE-T y 10GBASE-T 
Estos cables usan los 8 pares que contienen, que forman cuatro pares de hilos. 
- Se utilizan los mismos pares de pines 1-2 y 3-6 que en los cables 10BASE-T/1000BASE-T.
- Par de hilos en la posición 4-5
- Par de hilos en la posición 7-8

![[Pasted image 20241028012044.png]]
Cada par de hilos se puede usar para el Tx/Rx de forma simultanea. 

Si el tipo de cable es crossover, los pines 1-2 y 3-6 se cruzan de igual forma que en 10BASE-T/100BASE-T, los pines 4-5 y 7-8 tambien forman un crossover. 

> Por el Auto MDI-X, no es necesario preocuparse por estos detalles. 



