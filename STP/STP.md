---
tags:
  - STP
  - concept
---

# Spanning Tree Protocol

- Los _loops_ físicos siempre deben existir
- Los _loops_ lógicos deben ser controlados

## A considerar 

**Control de bucles en capa 3**
- En L3 el principal metodo para evitar los bucles dentro de la red, es el campo TTL (time to live, 0 to 255) que viene dentro del IP header. 
	- Por cada router por el que pasa el paquete se va restando el valor de TTL hasta llegar a 0, en ese caso de devuelve un mensaje de respuesta al dispositivo origen de tipo ICMP indicandole que el _TTL ha expirado_. 

**Control de bucles en capa 2**
- A diferencia de L3 con el campo TTL en IP header, en L2 no tenemos algun campo el cual tenga como fin evitar los bucles (ya sea por problemas en la MAC address, etc). Esta problemática es justamente lo que viene a resolver STP.

![](_anexos_/Screenshot%20from%202024-01-02%2005-54-16.png)

A considerar:
- El protocolo actual al que se denomina STP, es el que hace referencia a _IEEE 802.1D-2004_
	- No confundir con _IEEE 802.1D-1998_ (deprecated)
- Los protocolos en verde (en la imagen), son implementaciones de STP propietarias de Cisco. En cambio los violetas son implementaciones estándar definidos por la IEEE.

##### References
- https://www.networkplayroom.com/2017/10/quick-notes-spanning-tree-protocol-ccie.html