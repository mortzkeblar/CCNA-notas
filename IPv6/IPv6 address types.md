---
tags:
  - CCNA
---
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

Los LLAs son direcciones unicast usadas para identificar a un unico host dentro de un segmento de red. Los host que no se encuentran en el mismo segmento, no puede comunicarse usando LLAs. 

![[Pasted image 20241210235527.png]]

El rango definido para las LLA es `fe80::/10`. El standart establece que los siguientes 54 bits sean de solo 0s, por lo que el rango para las LLA queda en `fe80::/64`.

Los restantes 64 bits (el identificador de la interface) puede ser configurado usando [[IPv6#Modified EUI-64]]. Tambien se puede configurar manualmente la interface con `ipv6 address <address> link-local`.

![[Pasted image 20241211000443.png]]

El equivalente en [[IPv4 addressing|IPv4]] son los _IPv4 link-local_ addresses que tienen el rango `169.254.0.0/16`. La mayor diferencia es que el IPv6 link-local requiere que cada interface IPv6 tenga definida una LLA, en IPv4 no. 

## Multicast addresses 
Las direcciones multicast proporcionan comunicación one-to-multiples, de un host a varios destinos. Estos destinos son todos los hosts que se encuentren unidos a un _multicast group_ y acepten los mensajes multicast.

El rango definido para las direcciones multicast es `ff00::/8`. [[IPv6]] tiene definido varios _scopes_ para las direcciones multicast que depende de que tan lejos pueden ser enviados, estos scopes se determinan por el digito final del primer hextet. 

![[Pasted image 20241211012650.png]]

Representación visual del _multicast scopes_ desde la perspectiva de PC1

![[Pasted image 20241211012831.png]]

> El multicast scope que se considera para el CCNA es _link-local_, mensajes multicast que se envian a otros hosts en el mismo segmento. 

**Common link-local multicast addresses**

![[Pasted image 20241211020208.png]]

Se puede verificar a que grupos _multicast_ se encuentra asociado un interface en Cisco mediante el comando `show ipv6 interface <interface>`

![[Pasted image 20241211020421.png]]

## Anycast addresses
Cuando se usa una dirección unicast, la misma IP es compartida en multiples hosts. Estas direcciones son indistinguibles frenta a una dirección unicast cualquiera, ya que no hay un rango definido para estas. 
- Las unicast address son unicas y no se comparten entre hosts 
- Los anycast address no son unicas y pueden ser configuradas en multiples hosts 

Los paquetes enviados a una dirección anycast llegan a alguno de los hosts configurados, esto depende de que el  host sea el más cercano al host de origen.

![[Pasted image 20241211020707.png]]

> Anycast es muy usado en redes publicas, en la que se configuran la misma IP en multiples hosts de forma global para que los usuarios puedan acceder al server que se encuentre lo más cerca posible, usado en servicios _Content Delivery Network (CDN)_ como Cloudflare. 

Para configurar una dirección anycast se usa el comando `ipv6 address <address> anycast`, el keyword `anycast` se usa para etiquetar la IP, pero fuera de eso la dirección funciona de forma igual a una unicast address. 

![[Pasted image 20241211024338.png]]

## Other reserved addresses 
- La dirección IPv6 que contiene solo 0s, llamada _unspecified IPv6 address_ abreviada como `::`. Esta dirección puede ser usada como IP de origen cuando el router no sabe cual es su IP address. 
	- [[IPv6]] default route, esta se configura con destino `::/0`, equivalente en [[IPv4 addressing|IPv4]] a `0.0.0.0`
- Loopback [[IPv6]] address, es la dirección siguiente al _unspecified_ address. Se escribe como `::1`. 
	- Equivalente en IPv4 es el rango reservado `127.0.0.0/8`

