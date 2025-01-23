---
tags:
  - CCNA
---
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

## Retransmission 
_Retransmission_ es un concepto que usa cuando un segmento enviado no recibe una respuesta o _acknowledgement_ en un determinado periodo de tiempo, llamado _retransmission timeout_. Cuando ocurre esto es que se realiza el _retransmit_ del segmento. 

> _Retransmission_ junto a al _TCP acknowledgement_ es lo que proporciona la comunicación confiable en TCP.

![[Pasted image 20241219232323.png]]

Otro evento que genera un _retransmission_ es cuando un host recibe un segmento con un checksum defectuoso (similar a [[Ethernet Header (and Trailer)#Frame Check Sequence]]). TCP utiliza el campo _Checksum_ para comprobar si existen errores en los segmentos. 
- El valor del campo _checksum_ es calculado mediante un algoritmo que se envia en el TCP header, cuando el receptor recibe el segmento, calcula su propio valor _checksum_ a partir del segmento enviado. Una vez calculado realiza la comparación  con el valor _checksum_ enviado y en caso de no sean los mismos valores, el receptor no envía el _acknowledgement_, lo que hace que el emisor realice una _retransmission_.

## Flow Control 
_Flow control_ es una mecanismo de TCP para prevenir que el emisor no sature al receptor enviando demasiados datos muy rapidamente, asegurando que solo se envien la cantidad de datos que el receptor pueda procesar en un determinado momento.

La transferencia de datos adecuada depende de muchos factores como pueden ser la velocidad de conexión del receptor, la capacidad de procesamiento y la carga que tienen ambos. Esto es variable y depende de las condiciones dadas en un determinado momento.  

Si el receptor recibe más paquetes de los que puede procesar, esto resulta en un _dropped_ de segmentos, lo que genera muchas _retransmissions_ (malo para el rendimiento). 

Flow control es implementado en TCP usando el campo _Window Size_ del TCP header, esto le dice al emisor la cantidad de datos que debe enviar antes de esperar el _acknowledgement_.

![[Pasted image 20241220002940.png]]

En la imagen como simplificación solo se ve el _window size_ de PC1, en realidad cada host especifica su propio _window size_ en cada segmento que envia. 

> TCP ajusta dinamicamente su _window size_ para encontrar un ratio apropiado, por esto dice que TCP usa una _sliding window_.


