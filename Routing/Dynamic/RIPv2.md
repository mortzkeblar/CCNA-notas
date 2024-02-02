Routing Information Protocol Version 2 (RFC 2453) es la segunda versión del protocolo [RIP](../../../../../RIP.md), creado para superar las limitaciones de RIPv1.

- [RIPv2](RIPv2.md) ahora es un protocolo [classless](../../../../../classless.md) (entiende y trabaja sobre la información de las mascaras de red ([VLSM](../../../../../VLSM.md)) en las actualizaciones de routing). 
- Tambien soporta autenticación MD5
- A diferencia de RIPv1 que utilizaba mensajes broadcast, [RIPv2](RIPv2.md) ahora realiza actualizaciones multicast a `224.0.0.9` enviados solamente a otros dispositivos RIP.
- Es compatible con RIPv1, ambos pueden trabajar de forma conjunta. Además, se puede configurar cada interface para enviar/recibir actualizaciones de RIP que se necesite: `ip rip <receive or send> version <1 or 2>`


Al igual que RIPv1, utiliza el puerto UDP 520 y tiene una [administrative distances](administrative%20distances.md) de 120. 


## swap RIP version 
Por defecto al usar RIP, estas usando la versión 1. Esto se puede cambiar para usar [RIPv2](RIPv2.md) en la consola en modo config-router. 

``` bash
R1#conf t
R1(config)#router rip
R1(config-router)#version 2
R1(config-router)#network 10.0.0.0
```


