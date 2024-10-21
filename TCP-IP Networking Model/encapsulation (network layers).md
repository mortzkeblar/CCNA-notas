El concepto de encapsulación es el que permite al modelo de capas [[TCP-IP]] trabajar y comunicarse entre ellas. 

## Data encapsulation 
El proceso por el que un host puede enviar datos consiste en cinco pasos:
- Protocolo L7, prepara los datos para ser enviados 
- Protocolo L4, añade un _header_ a los datos a enviar
	> Un header son datos complementarios que se añaden al principio de los datos que van a ser transmitidos por la red. Contienen los datos usados por el protocolo que agrega el header (p. ej. un header L4 va a agregar el puerto de destino, etc)

- Protocolo L3, este agrega su propio header que contiene la IP de destino
- L2, este agrega su propio header además de un _trailer_
	> Un ethernet trailer es un dato complementario que se añade al final del mensaje a ser enviado, este contiene un bloque de datos usado para revisar si hubo errores en la transmisión del mensaje. (p. ej. interferencia electromagnetica)
	> 

- L1, finalmente una vez que los datos llegan a esta capa son enviados en forma de bits por un medio fisico, como un UTP cable. 

![[Pasted image 20241021112555.png]]

## Data de-encapsulation
Es el proceso contrario al de encapsulamiento, el host que recibe el mensaje inspecciona los datos de cada header/trailer y los va eliminando hasta llegar a los datos del mensaje. 
- El host de destino recibe el mensaje 
- Inspecciona el L2 header y trailer, los elimina y pasa el mensaje a L3
- Inspecciona el L3 header, los elimina y pasa el mensaje a L4
- Inspecciona el L4 header, los elimina y pasa el mensaje a aplicación correspondiente 
- La aplicación recibe el mensaje y procesa los datos 

![[Pasted image 20241021113034.png]]