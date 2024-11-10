---
tags:
  - CCNA
---
![[Pasted image 20241024231150.png|600]]

- El IPv4 header se debe leer de izquierda a derecha y de arriba hacia abajo. 
- El campo _options_ como indica su nombre es opcional
	- Sin este campo el header tiene un tamaño de 20 bytes 
	- Si el campo _options_ ocupa su tamaño maximo (40 bytes) el header tiene un tamaño total de 60 bytes


## The version field 
- 4 bits de tamaño 
- Indica la versión de IP que se esta usando 
	- value of 0b0100 (0d4) indicates IPv4 
	- value of 0b0110 (0d6) indicates IPv4.

## The IHL field 
_Internet Header Length_, tiene un tamaño de 4 bits. Y se usa para indicar el tamaño del IPv4 header, ya que es variable en tamaño dependiendo de si esta presente el campo _options_ (y el tamaño de este).

El IHL header indica el tamaño del IPv4 header en incrementos de 4 bytes. Si el valor del campo es 5, quiere decir que el tamaño del header es de 20 bytes.
-  Este campo tiene un valor minino de 5, ya que el IPv4 header tiene un tamaño minimo de 20 bytes 
- El valor maximo de este campo es 15, que indica que el tamaño del IPv4 header es de 60 byes 

## The DSCP and ECN fields 
_Differentiated Service Code Point (DSCP)_ y _Explicit Congestion (ECN)_ son dos campos de 6 y 2 bits de tamaño respectivamente. Tambien se puede encontrar en otras definiciones como un campo unificado llamado _Type of Service_.

Estos campos son usados para _Quality of Service (QoS)_, una característica usada que permite priorizar el envió de un determinado dato en el trafico de la red. 

## The Total Length field 
Este es un campo de 16 bits que indica el tamaño total del paquete. Que incluye el tamaño del IPv4 header (IHL field) más el payload. 

![[Pasted image 20241025002536.png|400]]
Los valores que se asignan en este campo están expresados en términos de bytes y no es necesario realizar conversiones como en el campo IHL.

## The Identification, Flags and Fragment Offset field 
Estos campos forman un tamaño total de 32 bits, son usados para la *fragmentación* de paquetes.

> IPv4 usa el concepto de _MTU (Maximum Transmission Unit)_ que indica el tamaño maximo que debe tener un paquete. 

El tamaño típico de un MTU es de 1500 bytes, si un dispositivo receptor no soporta ese tamaño lo que va a hacer es fragmentar el paquete. 

### Identification field 
- Tiene 16 bits de tamaño
- Se usa para identificar a que paquete originalmente pertenece un fragmento 
	- Cuando un paquete es fragmentado, todos los fragmentos del paquete tienen el mismo valor en este campo 

### Flags field 
Este campo de 3 bits se utiliza para el control y la identificación de fragmentos. Cada bit de este campo tiene un definición de uso:
- Bit 0 _Reservado_ - El uso de este bit no esta definido, por lo cual siempre esta seteado en 0
- Bit 1 _Don't Fragment (DF) bit_ - Si este bit tiene valor 1, significa que el paquete no debe ser fragmentado. 
	- Si el paquete es más grande que el MTU asignado, este se descarta 
- Bit 2 _More Fragment (MF) bit_ - Si este bit tiene valor 1, significa que aun quedan más fragmentos del paquete.
	- El fragmento final de un paquete tiene el valor 0 en este campo, que indica que no hay más fragmentos 
	- Los paquetes no fragmentados siempre tiene el valor 0 en este campo 

### Fragment Offset field 
Este campo de 13 bits se usa para indicar la posición del fragmento dentro del paquete original, esto permite volver a armar el paquete con los fragmentos incluso si estos llegan de forma desordenada. 
- Se puede dar el caso de que un destino que tiene múltiples rutas, cada fragmento tome una ruta diferente por lo cual es posible no lleguen de forma ordenada

## The TTL field 
_Time To Live_, es un campo de 8 bits.  
- Cuando un host envia un paquete, este se envia con un valor inicial en este campo (64 suele ser muy comun). Por cada router por el que pasa el paquete y este realiza el forwarding, el valor se decrementa en 1. 
	- Si el valor de este campo llega a 0, el router descarta el paquete.

Este mecanismo evita que los paquetes puedan formar _loops_ dentro de la red. 

![[Pasted image 20241025091950.png|500]]

## The Protocol field 
Este campo de 8 bits indica el tipo de mensaje que se encapsula dentro del paquete. Estos son algunos valores que puede tener este campo. 
- 1 - ICMP 
- 6 - Transmission Control Protocol ([[TCP-IP]])
- 17 - User Datagram Protocol ([[UDP]])
- 89 - Open Shortest Path First ([[OSPF]])

## The Header Checksum field 
Este campo de 16 bits se usa para detectar errores en el IPv4 header. Este es un campo similar al campo FCS en [[Ethernet Header (and Trailer)]]. 
- La principal diferencia entre Header Checksum field y FCS es que el primero solo busca errores en el IPv4 header, no en todo el paquete. A diferencia de FCS que realiza la busqueda de errores en todo el frame

## The Source/Destination Address field 
Estos campos tiene 32 bits de tamaño y como su nombre indica contienen la IP address de origen y destino. 

## The Options field 
Este es un campo opcional y variable en tamaño, desde 0 bytes (no se usa) hasta 40 bytes de tamaño. 