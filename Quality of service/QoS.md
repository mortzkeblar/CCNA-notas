**_Quality of Service (QoS)_** es un conjunto de tecnologías que permiten realizar la _priorization_ del trafico más importante (y la _de-priorization_ del menos importante). Esto resulta especialmente relevante cuando se este en un contexto de congestión en la red, y se necesita garantizar la calidad en trafico sensible como [[IP telephony]]. 

## Bandwidth
El _bandwidth_ hace referencia a la capacidad del enlace, cuantos datos puede _carry per second_. 

![[Pasted image 20250314020552.png]]
En una situación normal, generalmente los host que se conectan a la red tienen enlaces `GigabitEthernet` que les permite tener bastante capacidad, en cambio los enlaces WAN suelen ser un potencial _bottleneck_ debido al costo. 

En una situación de alto consumo de un enlace, se pueden presentar problemas como el _delay_, _jitter_ o _loss_. 

## Delay, jitter and loss 
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

### IP Service-Level Agreement (SLA)
IP SLA es una función que se puede usar para medir aspectos del rendimiento de la red, como el delay, RTT, jitter y loss.

Un _Service-Level Agreement_ es un conjunto predefinido de métricas/normas de rendimiento que se espera que cumpla un servicio.

En Cisco IOS, IP SLA ayuda a verificar estas métricas usando mecanismos como pruebas ICMP echo request (ping) para medir el RTT y loss, que luego se puede usar para medir la efectividad de las politicas de [[QoS]]. 