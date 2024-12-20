---
tags:
  - CCNA
date created: Monday, October 21st 2024, 12:04:56 pm
date modified: Wednesday, November 20th 2024, 11:33:50 pm
---
**UDP (User Diagram Protocol)** es un protocolo [[Layer 4]], que proporciona direccionamiento y [[Layer 4#Session multiplexing]] a través de los port numbers. UDP se centra en enviar el trafico al maximo esfuerzo posible, no tiene un control de flujo como TCP ni garantiza la fiabilidad de los datos enviados. Por lo cual se le considera un protocolo *no orientado a conexión*.
- UDP al ser _non connection-oriented_ significa que no se establece una conexión entre los hosts como [[TCP]], el host simplemente envia los datos.
- UDP no proporciona _data sequencing_ para asegurar que los segmentos de datos se procesen en el orden correcto y sin perdidas. 
- El host receptor no envía *acknowledgments* hacia el emisor por si existen fallas en el envío. 
- El control de errores va a depender de los protocolos que estén corriendo en las *capas superiores*
- UDP no proporciona _flow control_, esto implica que UDP envia los datos tan rapido como se pueda. Sin asegurar si el receptor tiene o no la capacidad de procesamiento. 


Mientras que el [[TCP]] header puede ocupar entre 20-60 bytes de tamaño, el UDP header tiene un tamaño de solo 8-bytes. 

![[Pasted image 20241220003524.png]]

### Datagrams and data streams 
Una _datagrama_ es una unidad basica de transferencia que es independiente de otros mensajes. UDP usa un _datagram model_ para la transmisión, lo que significa que envia datos en _discrete chunks_ o fragmentos discretos (datagrams) que son independientes entre sí.  

Esto se diferencia de [[TCP]], que trata los datos como un _continuous stream_. Este flujo se divide en fragmentos manejables (segments) para su transmisión. Pero la división de estos segmentos no se hace en relación a la propia estructura del dato. 

### Comparing TCP and UDP 
[[TCP]] añade una sobrecarga adicional a comparación de [[UDP]], esto se puede ver en:
- _Data overhead_ - TCP header tiene un tamaño de 20-60 bytes contra los 8-bytes de UDP, lo que proporciona mayor espacio para el _user data_.
- _Processing overhead_ - hace referencia a la carga computacional adicional para procesar [[TCP]], debido a todas las caracteristicas adicionales. 
- _Time overhead_ - establecer conexiones [[TCP]] al igual que la espera de los _acknowledgments_ puede generar latencia adicional y una perdida de rendimiento en comparación con UDP.

![[Pasted image 20241220022209.png]]

[[TCP]] se prefiere en casos donde la integridad de los datos es critica:
- File transfers 
- Web browsing (HTTP/s)
- Email protocols like SMTP, POP3 and IMAP 

UDP es simple y ligero, se prefiere en casos como:
- Real-time applications - video streaming, voice/video calling
- Simple query/response protocols - i.e. DNS 
- Si la _confiabilidad_ es proporcionada por otros medios (i.e. protocolos en capas superiores) - i.e. TFTP 

![[Pasted image 20241220022927.png]]
