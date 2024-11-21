---
tags:
  - CCNA
date created: Monday, October 21st 2024, 12:04:56 pm
date modified: Wednesday, November 20th 2024, 11:33:50 pm
---
El modelo TCP/IP fue creado por el US Departament of Defense, luego definida en el RFC 1122. Originalmente fue definida en cuatro capas, pero la versión más utilizada es la de cinco capaz haciendo similitudes con el modelo OSI.

Estas capas trabajan mediante juntas haciendo uso del proceso de [[encapsulation (network layers)]]. 

![[Pasted image 20241021022634.png]]


Es un modelo orientada a conexión, una vez que la conexión esta establecida, los datos pueden ser enviadas unidireccionalmente sobre esa conexión.
- TCP realiza secuenciación para asegurar que los segmentos de datos se procesen en el orden correcto y no haya perdidas.
- TCP is confiable, el receptor envía acknowledgments hacia el emisor. Informa si hay segmentos perdidos en base a los números de secuencia y estos son reenviados.

![[Pasted image 20241011204829.png|400]]
> L4PDU

## adjacent-layer and same-layer interactions 

En el modelo TCP/IP, cada capa proporciona un servicio para la capa superior. Esto es llamado _adjacent-layer interaction_.
- L4 proporciona un servicio a L7 entregando los datos a la aplicación apropiada en el host de destino 
- L3 proporciona un servicio a L4 entregando los _segments_ al host de destino correspondiente 
- L2 proporciona un servicio a L3 entregando los _packets_ al next-hop
- L1 proporciona un servicio a L2 proporcionando el medio fisico a través del cual viajan los _frames_

Otro concepto relacionado es el de _same-layer interactions_. Hace referencia a las comunicaciones entre la misma capa en diferentes computadores. 
- Los datos de aplicación son enviados desde un host a una aplicación otro host 
- Cuando los datos se encapsulan con el L4 header, el segmento es direccionado hacia L4 del host de destino 
- Cuando un segmento se encapsula con un L3 header, el paquete es dirigido hacia L3 del host de destino
- Cuando un paquete se encapsula con un L2 header/trailer, el frame es dirigido hacia L2 del next-hop 
- Las señales enviadas por un puerto fisico de un dispositivo son recibidos por el puerto de otro dispositivo

![[Pasted image 20241021115736.png]]

