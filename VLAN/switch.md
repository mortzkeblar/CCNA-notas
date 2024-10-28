---
tags:
  - CCNA
---
Un switch es un dispositivo de red que opera en la capa 2 del modelo OSI (capa de enlace de datos). Su función principal es interconectar dispositivos en la misma red local (LAN) y facilitar la comunicación entre ellos.

Los switches suelen realizar principalmente estes funciones:
- Learning MAC addresses 
- Filtering and forwarding frames 
- Preventing loops in the network

### learning MAC addresses 
Ver: [[MAC address table]] 
### filtering and forwarding frames 
Cuando un frame llega a un switch, este revisa la dirección de destino del frame y revisa si se encuentra en su database. 
- Si la dirección esta en la tabla, el frame se envia fuera de la interface a la que esta conectado el host de destino, esto se conoce como frame filtering 
- Si la dirección no esta en la tabla, el switch no tiene otra opcion que inundar el frame desde hacia todos los puertos (excepto del que llego)
### preventing loops in the network
Tener multiples rutas hacia los destinos pueden provocar loops si se envian broadcast desde un enlace, esto se conoce tambien como _storn difusion_.

![[_anexos_/Pasted image 20240519223611.png|500]]

Esto se puede prevenir mediante [[L2/STP/STP]]. 

### relacionados
- [MAC address table](../Ethernet%20LAN%20switching/MAC%20address%20table.md)
- [collision domain](collision%20domain.md)
- [[Project/Networking/CCNA-notas/Ethernet LAN switching/Frame switching|Frame switching]] 
