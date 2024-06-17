---
tags:
  - routing
  - protocol
  - CCNA
---

Address Resolution Protocol (ARP) es un protocolo de red usado para encontrar la dirección MAC de un dispositivo a través de su dirección IP. 

Las ARP request paquets se envian por mensajes broadcast (FF:FF:FF:FF:FF:FF para el broadcast ethernet, 255.255.255.255 para los mensajes IP broadcast).

Los SO mantiene un registro ARP, llamado cache ARP que suele permanecer unos minutos y que se verifica antes de lanzar un broadcast para ver si ya tiene la IP que necesita. Para poder ver el cache se puede utilizar `arp`. 
``` bash
arp -a
```