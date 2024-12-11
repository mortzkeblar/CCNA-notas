Al igual que [[IPv4 addressing|IPv4]], en [[IPv6]] tenemos rangos de direcciones reservadas para propositos especificos.
- **Global unicast** - direcciones globalmente unicas que se usan para la comunicación sobre internet 
- **Unique local** - direcciones que no son globalmente unicas, puede ser usada para comunicaciones internas pero no sobre internet. 
- **Link-local** - usada para comunicación entre hosts conectados directamente 
- **Multicast** - usada para la comunicación one-to-multiples, permite que un paquete singular pueda ser direccionada a multiples hosts 
- **Anycast** - usada para comunicación _one-to-one-of-multiple_, esta es una dirección unicast que es asignada a multiples hosts. Los paquetes se entragan al host más cercano que tiene configurado esa dirección
	- Este tipo de direcciones es usado para la comunicación _low latency_

![[Pasted image 20241209235852.png]]

## Global unicast 
Son direcciones globalmente unicas que se usan para la comunicación sobre el internet publico, estas direcciones son contraladas por la IANA. 

El rango definido originalmente para las direcciones global unicast es `2000::/3`, esto incluye desde la dirección `2000::` hasta la `3fff:ffff:ffff:ffff:ffff:ffff:ffff:ffff`. Sin embargo este tipo de direcciones se extendió más alla del rango definido originalmente.

A las empresas se les asigna tipicamente un bloque de direcciones `/48`, llamado tambien _global routing prefix_. 
- Como el bloque de direcciones más comun son `/64`, tener `/48` te da 16 bits disponibles para poder generar las subredes.
	- Los 16 bits disponibles son llamados tambien _subnet identifier_
	- Los restantes 64 bits disponibles forman parte de la porción de host 

![[Pasted image 20241210091150.png]]
El [[Subnetting]] en [[IPv6]] es igual que en IPv4, en este caso se suelen tomar prestados los bits de la _subnet identifier_ para poder crear otros bloques de red. 

![[Pasted image 20241210091547.png]]

## Unique local 
**[[IPv6]] Unique Local addresses (ULAs)** son direcciones de uso privado usado que estan libres para ser usados dentro de una red interna, no son globalmente unicos. 

El rango definido para las ULAs es `fc00::/7`, esto incluye las direcciones desde `fc00::` hasta `fdff:ffff:ffff:ffff:ffff:ffff:ffff:ffff`. A su vez este rango es dividido en dos bloques `/8`
- `fc00::/8` - actualmente reservado y no definido para ningun proposito
- `fd00::/8` - el rango activo que se puede usar como [[IPv6]] ULAs 

![[Pasted image 20241210092141.png]]
Luego del prefijo `fd`, los ULAs tienen el campo de 40 bits _Global ID_, este es generado de forma aleatoria. En la practica esto permite crear direcciones ULAs únicas. 

## Link-local 
**[[IPv6]] Link-local addresses (LLA)**, estas se generan automaticamente al habilitar [[IPv6]] en una interface. 

> Un Link-local address puede ser generado incluso sin que la interface tenga configurada una [[IPv6]] address, basta con que se haya ejecutado `ipv6 enable` en la interface config. 


