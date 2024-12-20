---
tags:
  - CCNA
date created: Monday, October 21st 2024, 12:04:56 pm
date modified: Wednesday, November 20th 2024, 11:33:50 pm
---
**The transport layer**, esta capa se encarga de enviar los datos a un proceso de una aplicación (session multiplexing) dentro del host de destino una vez que los datos hayan llegado a su destino mediante L2/L3. Al igual que L2 (MAC address) y L3 (IP address), L4 tambien tiene su propio esquema de direccionamiento llamado _port numbers_.

![[Pasted image 20241021105334.png]]

Tambien es responsable de la recuperación de error end-to-end y el control de flujo. El control de flujo hace referencia al proceso de ajustar el flujo de los datos enviados para asegurar que el receptor pueda procesar esos datos correctamente.

Los protocolos más usados dentro de L4 son [[TCP]] y [[UDP]].
## Port numbers 
Los _ports_ son el sistema de direccionamiento de los protocolos [[Layer 4]], estos cubren el rango `[0-65535)`. Los puertos de destino se usan para identificar el protocolo de capa superior. 
- La combinación de puertos de origen y destino pueden ser usada para trackear las sesiones.

![[Pasted image 20241216020405.png]]
- Cuando un server ejecuta una application service se dice que "_listen on_" en un puerto particular, que se encuentra a la espera de conexiones dirigidas a ese puerto 

![[Pasted image 20241216232153.png]]

Los números de puertos son asignados por la IANA, esta las segmenta en tres rangos con diferentes propositos. 
- **Well-know ports** - `[0, 1023]`
	- Reservado para los protocolos de uso comunes 
	- Controlado y asignado por la IANA 
	- Tambien llamados _system ports_
- **Registered ports** - `[1024, 49151]`
	- En este rango se puede registrar protocolos particulares, el registro esta a cargo de IANA 
		- El registro se hace para evitar conflictos, como que dos servicios corran en el mismo puerto 
	- Tambien llamado _user ports_
- **Epheremal ports** - `[49152, 65535]`
	- No esta controlado ni asignado por la IANA 
	- Se usa de forma dinamica por los dispositivos clientes como el _source port_ de una conexión 
	- Tambien llamada _dynamic_ o _private ports_


## Session multiplexing 
Es el proceso donde un host es capaz de soportar múltiples sesiones simultáneamente y manejar el tráfico individual sobre un único enlace. 
- El _source port_ es un puerto aleatorio que se encuentra en el rango de _epheremal ports_ y que sirve como identificar de la sesión 

![[Pasted image 20241217010005.png]]

La combinación de [[IP address]], un [[#Port numbers]] y un protocolo [[Layer 4]] es llamado un **socket**. Se puede combinar una _session_ como la combinación de dos sockets.
- _Client socket_ - (i.e) 192.168.1.1, TCP port 5000
- _Server socket_ - (i.e) 203.0.113.1, TCP port 443 

Tambien es llamado _five-tuple_ por los elementos que lo componen: IP de origen/destion, puerto de origen/destino y el protocolo de capa 4. 

