**_Quality of Service (QoS)_** es un conjunto de tecnologías que permiten realizar la _priorization_ del trafico más importante (y la _de-priorization_ del menos importante). Esto resulta especialmente relevante cuando se este en un contexto de congestión en la red, y se necesita garantizar la calidad en trafico sensible como [[IP telephony]]. 

## QoS concepts
### Bandwidth
El _bandwidth_ hace referencia a la capacidad del enlace, cuantos datos puede _carry per second_. 

![[Pasted image 20250314020552.png]]
En una situación normal, generalmente los host que se conectan a la red tienen enlaces `GigabitEthernet` que les permite tener bastante capacidad, en cambio los enlaces WAN suelen ser un potencial _bottleneck_ debido al costo. 

En una situación de alto consumo de un enlace, se pueden presentar problemas relacionado con el _delay_, _jitter_ o _loss_. 

### Delay, jitter and loss 
- _Delay (or latency)_ es la cantidad de tiempo que toma un paquete en viajar desde el origen hacia el destino a través de la red
	- _Round-Trip Time (RTT)_ es un concepto relacionado que muestra el delay de forma bidireccional, es la cantidad de tiempo que toma en viajar al destino y que el paquete de respuesta llegue de vuelta al origen.
	- El _delay_ se mide generalmente en milisegundos (ms), Cisco recomiendo que el trafico de [[IP telephony]] tenga un delay menor o igual a 150ms. 

![[Pasted image 20250314021715.png]]


- _Jitter_ es la variación del _delay_ en una serie de paquetes. Si bien el _delay_ es inevitable en una comunición, se espera que esos valores sean minimos y se mantengan constantes. Tener valores altos de jitter afectan significativamente la experiencia de las app real-time. 
	- El _jitter_ se mide generalmente en milisegundos (ms), Cisco recomienda que el trafico de [[IP telephony]] tenga un jitter menor o igual a 30ms.

![[Pasted image 20250314022635.png]]

- _Loss_ es el porcentaje de paquetes que se perdieron (que no llegaron a destino) en un periodo de tiempo. En una situación normal los valores optimos serian menores a 0%, cuando se tiene congestión es posible que algunos paquetes comienzen a ser droppeados. 
	- Cisco recomienda un _loss_ menor al 1% para [[IP telephony]] 
	- Las aplicaciones basadas en [[TCP]] tiene la capacidad de lidiar con los _dropped packets_ a través del  [[TCP#Retransmission]]
	- Los protocolos VoIP basados en [[UDP]] no pueden hacer el _retransmission_ de los _dropped packets_. 
		- Esto es intencional ya que al ser aplicaciones _real-time_ se prioriza que tenga un menor _delay_, pero se espera que no haya demasiados _drops_ que puedan comenzar a afectar la experiencia del usuario  

#### IP Service-Level Agreement (SLA)
IP SLA es una función que se puede usar para medir aspectos del rendimiento de la red, como el delay, RTT, jitter y loss.

Un _Service-Level Agreement_ es un conjunto predefinido de métricas/normas de rendimiento que se espera que cumpla un servicio.

En Cisco IOS, IP SLA ayuda a verificar estas métricas usando mecanismos como pruebas ICMP echo request (ping) para medir el RTT y loss, que luego se puede usar para medir la efectividad de las politicas de [[QoS]]. 


## Classification and marking
_Classification_ es el proceso de clasificar los mensajes dentro de _classes_ basados en el tipo de tratamiento que se les desea dar: high priority, low priority, etc. 

_Marking_ es el proceso de configurar el valor de ciertos campos del _header_ para simplificar el proceso de _classification_
### Classification 
_Classification_ permite priorizar cierto tipo de trafico identificandolo y separando los paquetes dentro de cuatro clases diferentes:
- _Real-time_ - como trafico de voz o video 
- _Business critical_ 
- _Default_
- _Scavenger_ - es trafico _low-priority_, en un contexto de congestión es este trafico el primero en ser droppeado o bien aumente el delay en pos de priorizar el trafico más prioritario

![[Pasted image 20250315232114.png]]

 Los [[1000 Project/1100 Networking and Security/1110 CCNA-notas/Access Control Lists/_LEGACY - ACL_/ACL|ACL]] son una forma de clasificar los paquetes, los paquetes _permitted_ por un ACL son asignados a una clase y así sucesivamente, en este caso existe la limitación de solo puede hacer matching con información basada en los campos de headers de [[Layer 3]] y [[Layer 4]].

Para determinar de forma más profunda el tipo de paquete que este siendo enviado, se puede usar mecanismos de clasificación más sofisticados como _Network-Based Application Recognition (NBAR)_ que permite realizar una _deep packet inspection_.

### Marking 
El proceso de _marking_ se puede realizar basado en [[1000 Project/1100 Networking and Security/1110 CCNA-notas/Access Control Lists/_LEGACY - ACL_/ACL|ACL]] o usando NBAR.
- [[1000 Project/1100 Networking and Security/1110 CCNA-notas/Access Control Lists/_LEGACY - ACL_/ACL|ACL]] - el paquete se puede examinar basandose en direcciones de [[Layer 3]] / [[Layer 4]] 
- Network-Based Application Recognition (NBAR) - el paquete se puede examinar basandose en el contenido del _payload_

La clasificación mas compleja usando NBAR / [[1000 Project/1100 Networking and Security/1110 CCNA-notas/Access Control Lists/_LEGACY - ACL_/ACL|ACL]] puede ser demandante en terminos de recursos para el dispositivo, además que implicaría mayor complejidad al tener que configurar en cada dispositivo de red. 

Lo ideal es que los paquetes deberian ser clasificados lo antes posible y luego ser _marked_ (usando campos específicos del [[IPv4 Header]] / [[Ethernet Header (and Trailer)]]) para que el proceso de clasificación sea más simple en el resto de la vida del paquete. Los campos usados para el _marking_ son:
- IPv4 / [[IPv6]] : _Differentiated Services Code Point (DSCP)_
- Ethernet: _Priority Code Point (PCP)_, campo que se encuentra en el [[IEEE 802.1Q]] tag 

![[Pasted image 20250316001518.png]]
Una vez que los paquetes son _marked_, ya no es necesario realizar el mismo proceso sino que los dispositivos por donde pase el paquete van a revisar el _marking_ en los campos DSCP / PCP y en base a eso tomar la decisión de donde va clasificado el paquete. 

> El campo PCP tambien es llamado _Class of Service (CoS)_, es importante notar que _CoS_ solo hace referencia al campo ubicado en el _802.1Q_ tag. 

##### PCP field
Los tres bits del campo _PCP_ pueden ser usados para _marked_ frames ethernet de tipo 802.1Q-tagged, permitiendo la clasificación en función del valor de este campo. 

![[Pasted image 20250316004014.png]]
El campo PCP proporciona $2^{3} = 8$ valores posibles, los cuales son usados para el _standard PCP marking_ que clasifica el trafico en función de su prioridad. 

El valor por defecto es _best effort = 0_ que significa que no tiene ningún tratamiento de [[QoS]] aplicado.

![[Pasted image 20250316004348.png]]

Como el campo _PCP_ forma parte del [[IEEE 802.1Q]] tag, eso implica que solo se puede aplicar sobre los enlaces troncales, los frames enviados entre dos routers por ejemplo no reconocen el _tagged_ lo que resulta en un gran limitación debido a que el [[QoS]] no se consideraría en todos los puntos por donde viaja un paquete como muestra la siguiente imagen. 

![[Pasted image 20250316102911.png]]
#### Marking the DSCP field 
Tanto [[IPv4 Header]]  como [[IPv6]] contienen el campo de 6 bits _DSCP_:
- En IPv4, es parte de 1 byte compuesto del [[IPv4 Header#DSCP and ECN fields]] llamados _Differentiated Services (DiffServ)_. Anteriormente tambien se los conocia como _Type of Service (ToS)_
- En IPv6, es parte de 1 bytes llamado _Traffic Class_, al igual que IPv4 se componen de un [[IPv4 Header#DSCP and ECN fields]]

Anteriormente solo se usaban un campo de 3-bits para esta función llamada _IP Precedence (IPP)_ y que formaba parte del _ToS_.

> El campo _Explicit Congestion Notification (ECN)_ tiene el proposito de notificar al receptor del paquete sobre la congestión en la red, sin embargo la adopción de este campo es limitada. 

![[Pasted image 20250316104632.png]]

La mayor ventaja de usar _DSCP marking_ frente a _PCP marking_ es que el primero viaja con el paquete de forma _end-to-end_ (en relación a la definición de [[Layer 3]]), por lo que una vez el paquete es _marking_ va a permanecer así hasta llegar al destino. 

Otra ventaja del _DSCP marking_ frente a _PCP_ y _IPP_ es que su campo es de 6-bits, lo que permite $2^{6} = 64$ valores únicos que se pueden usar para aplicar los políticas de [[QoS]]. 

El propósito que cumplen cada uno de estos valores esta definido de forma estándar, existen dos conjuntos de DSCP marking estandarizados: _Class Selector_ y _Assured Forwarding_

##### Class Selector 
_Class Selector_ es un conjunto standard de DSCP marking creado para ser retrocompatible con el sistema de marking de IPP, por lo que solo usa los primeros 3-bits del _ToS/DiffServ byte_ al igual que IPP, los ultimos 3-bits menos significativos se dejan seteados en $0$.  

![[Pasted image 20250316112125.png]]

Como los campos usados para los valores _CS_ son los primeros bits más significativos, esto implica realizar una traducción cuando se quiera ver el valor total del campo DSCP incluyendo a los bits menos significativos. Por ejemplo el valor $7$ IPP es equivalente a $CS7$ ($0b111$) que es equivalente al valor del campo DSCP $56$ ($0b111000$).

Si bien el objetivo de _CS_ es ser retrocompatible con sistema IPP, tiene como desventaja que no aprovecha las valores adicionales que se agregan el campo DSCP.
##### Assured Forwarding 
_Assured Forwarding (AF)_ define 12 valores de marking estandarizados adicionales: cuatro _traffic classes_, y dentro de cada clase se puede encontrar tres niveles de _drop precedence_.

> Un paquete marcado con un _high drop precedence_ es más probable que termine droppeado en un contexto de congestión en la red 

Los paquetes son asignados a uno de los cuatro _queues_, dentro de cada queue algunos paquetes tienen más probabilidades de ser dropped que otros. 

Assured Forwarding (AF) usa los primeros 5-bits más significativos del campo DSCP, el último bit menos significativo tiene siempre el valor $0$. 

Los 5-bits se dividen a su vez dentro de un _class value_ (3-bits) y un _drop precedence_ (2-bits), estos se escriben como `AF<X><Y>` (X = class, Y = drop precedence)

![[Pasted image 20250316201533.png]]

**The 12 AF marking table (decimal DSCP values)**
![[Pasted image 20250316201923.png|400]]

Un shortcut para calcular el valor decimal del campo DSCP para un AF particular, es con la formula $(class*8)+(drop\ precedence*2)=decimal\ DSCP\ value$.

###### RFC 4594 recommended markings
![[Pasted image 20250316202522.png]]

El _RFC 4594_ incluye varias recomendaciones sobre las valores que se deben usar para los distintos tipos de trafico, es importante aclarar que no se debe hacer uso de todas sino de las que sean necesarias según nuestro criterio. 

### Trust boundaries 
Un _trust boundary_ es una división lógica en una red, que establece los dispositivos de _confianza_ que realizan el _marking_:
- Si un dispositivo de confianza realizo el marking de un paquete, este es reenviado por la red sin modificar el marking 
- Si un dispositivo que no es de confianza realiza el marking de un paquete, este es _re-mark_ de acuerdo a los políticas configuradas. 

![[Pasted image 20250316203424.png]]
Los [[IP telephony]] realizan el _marking_ de sus propios paquetes (los configuran en _EF_), se establece como standard que ese marking es de confianza o _trust_, en cambio el marking que puede realizar un PC particular generalmente se toma como _untrusted_, por lo que se realiza el _re-mark_ que puede ser hecho tanto por un teléfono como por un [[switch]].

## Queuing and scheduling 
_Queuing_ es el proceso de almacenar paquetes en queues (o colas) mientras esperan su turno a ser transmitidos, esto ocurre en un contexto de congestión de la red cuando una interface recibe paquetes a un mayor ritmo del que puede reenviarlos. 

Cuando no hay QoS, cada interface usa un  _single egress queue_ y el dispositivo transmite los paquetes mediante la forma _FIFO (first in, first out)_, lo que implica que los paquetes ingresan al queue en orden de llegada, los metodos en que los paquetes se ordenan es llamado _scheduling_.

> _Scheduling_ es el proceso de decisión sobre en que orden deben ser transmitidos los paquetes que estan dentro del queue. 

FIFO es el método más simple y es default en redes sin [[QoS]], cuando se necesita Quality of Service se utilizan otras técnicas de queuing / scheduling como: _Priority Queuing (PQ)_, _Class-Based Weighted Fair Queuing (CBWFQ)_ y _Low Latency Queuing (LLQ)_.

### Priority Queuing (PQ)
Este método soporta cuatro _queues_ separados que son clasificados por prioridad. Los paquetes se clasifican en _markings_, los que estén en los _low-priority queues_ seran transmitidas por el scheduler solo sí los _higher-priority queues_ estan vacias.

![[Pasted image 20250316214256.png]]

> Un scheduling realiza un _service a queue_ cuando reenvia un paquete (o varios) de ese queue 

Cuando el _PQ scheduler_ decide cual queue va a ser servido, este escanea los queues en orden de prioridad. Esto se realiza una vez por cada paquete, lo que significa que si el queue de mayor prioridad siempre esta recibiendo paquetes no permite que los queues de menor prioridad sean atendidos con menor frecuencia, esto es llamado _queue starvation_. 

Debido al queue starvation, hace que PQ sea un opción muy poco usada a pesar que sea mejor que el FIFO simple. 

### Class-Based Weighted Fair Queuing 

