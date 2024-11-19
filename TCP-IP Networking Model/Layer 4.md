---
tags:
  - CCNA
---
**The transport layer**, esta capa se encarga de enviar los datos a un proceso de una aplicación (session multiplexing) dentro del host de destino una vez que los datos hayan llegado a su destino mediante L2/L3. Al igual que L2 (MAC address) y L3 (IP address), L4 tambien tiene su propio esquema de direccionamiento llamado _port numbers_.

![[Pasted image 20241021105334.png]]

Tambien es responsable de la recuperación de error end-to-end y el control de flujo. El control de flujo hace referencia al proceso de ajustar el flujo de los datos enviados para asegurar que el receptor pueda procesar esos datos correctamente.

Los protocolos más usados dentro de L4 son [[TCP-IP]] y [[UDP]].
### Session multiplexing 
Es el proceso donde un host es capaz de soportar múltiples sesiones simultáneamente y manejar el tráfico individual sobre un único enlace. 

### Port numbers 
Los puertos de destino L4 se usan para identificar el protocolo de capa superior. 
- La combinación de puertos de origen y destino pueden ser usada para trackear las sesiones.



