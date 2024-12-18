**Transmission Control Protocol (TCP)**, es un un protocolo de tipo [[Layer 4]].

Ademas de proporcionar direccionamiento y [[Layer 4#Session multiplexing]], tiene caracteristicas destacadas como _connection-oriented communication_, data sequencing, comunicación confiable por medio del _acknowledgement_ y _retransmission_, y flow control.

**TCP header**
![[Pasted image 20241218000752.png]]

El tamaño de TCP header, al igual que el [[IPv4 Header]] tiene un tamaño minimo de 20-bytes, extendible hasta 60-bytes dependiendo si se incluye el campo _options_.

> La rango de puertos `[0-65535]` se debe al tamaño de los campos _source / destination ports_ de 16 bits, por lo que $2^{16}-=65.536$

## TCP connections 
Que TCP sea un protocolo _connection-oriented_ quiere decir que para el intercambio de datos, primero debe establecerse una conexión, así como tambien luego de intercambiar los datos, los hosts deben cerrar la conexión.

Para esto TCP tiene el three-step process:
- Establecimiento de la conexión 
- Intercambio de datos 
- Terminación de la conexión 

El campo _flags_ de 8-bits, lo que le permite tener 8 _flags_ diferentes, estos se usan para establecer indicaciones dependiendo del valor de flag que tenga en ese momento.

Las conexiones TCP se establecen usando dos flags en el TCP header: 
- _SYN (Synchronize)_
- _ACK (Acknowledgement)_

![[Pasted image 20241218014529.png]]

Este proceso es llamado **Three-way handshake process**
- Los tres segmentos enviados no contienen un payload con data ya que solo se usa para el establecimiento de la conexión TCP 
	- Una vez que el conexión TCP fue establecida, se comienza a intercambiar datos entre los hosts 

Una vez que se realiza el intercambio de datos, la conexión finaliza mediante el proceso _four-way handshake_. En el que se hace uso de los flags _FIN (Finish)_ y _ACK (Acknowledgement)_.

![[Pasted image 20241218014539.png]]

> Si bien el _TCP connection estableshment_ siempre se realiza de forma secuencial, en _connection termination_ no necesariamente ocurre lo mismo. 

## Data sequencing and acknowledgement
_Data sequencing_ es una funcionalidad de TCP que permite reorganizar los segmentos de forma ordenada en caso de que estos hayan llegado a destino de forma desordenada. 

Para esto se usa el campo _Sequence Number_ en el TCP header, además cada segmento enviado es confirmado por el receptor usando el campo _Acknowledgement Number_, lo que proporciona una _reliable communication_. 

![[Pasted image 20241218022014.png]]

Durante el _TCP connection estableshment_, cada host establece un _initial sequence number_ aleatorio que se va incrementando a medida que se envian mensajes al otro host. En el ejemplo anterior, PC1 uso el `seq=10` mientras que SRV1 usa el `seq=50`.

Para generar el _acknowledgement_ de un segmento particular, el host receptor establece el _acknowledgement number_ con el valor de _sequence number_ que espera recibir en el siguiente segmento. En el ejemplo anterior, PC1 recibe el segmento de SRV1 con `seq=50`, entonces PC1 realiza la confirmación con `ack=51`. 

En el ejemplo anterior, los segmentos se enviaban y recibian mediante turnos, un segmento a la vez. Aunque por eficiencia, es posible reconocer multiples segmentos con un solo segmento _acknowledgement_. 


![[Pasted image 20241218030425.png]]

Las conexiones TCP operan de forma bidireccional, cada host mantiene su propio _sequence number_ y _acknowledgement number_. El incremento de estos número esta determinado por la dinamica de la comunicación entre los hosts. Por ejemplo en una situación de transferencia de archivos de un host a otro, solo aumentaran de forma consistente el _number sequence_ del remitente y el _acknowledgement number_ del  receptor. 

> Es importante aclarar que los valores tanto de _acknowledgement_ como de _sequence_ son en forma de los bytes que envian y reciben. No son valores incrementales de a 1. 