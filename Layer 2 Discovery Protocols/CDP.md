---
tags:
  - CCNA
---
**Cisco Discovery Protocol (CDP)** es un protocolo propietario de Cisco de tipo layer-2, este se encuentra habilitado por defecto en los [[switch]]es Cisco sin necesidad de más configuraciones. 

![[Pasted image 20250114055526.png|400]]

> Los mensajes CDP se envian a las MAC address multicast `0100.0CCC.CCCC`, cuando un switch recibe el mensajes no realiza el flooding sino que lo conserva para sí mismo.

Para ver a los CDP neighbors se puede usar el comando `show cdp neighbors`.

![[Pasted image 20250114060641.png]]

- `local interface` - lista la interface en el dispositivo local donde esta conectado el vecino. 
- `port ID` - lista la interface del vecino donde se conecta nuestro dispositivo local.
- `holdtime` - el tiempo de espera hasta que se elimine un vecino CDP por falta de respuesta, valor por defecto en 180. Cuando se recibe un mensaje CDP, este valor se restablece nuevamente a 180.
- `capability` - identifica el tipo de dispositivo 
- `platform` - indica el modelo de hardware del vecino 

Tambien se puede usar `show cdp neighbors detail` para obtener más detalles, si se quiere buscar por un dispositivo especifico se usa `show cdp entry <device>`

![[Pasted image 20250114061518.png]]

## Configuring and verifying CDP 
Para verificar que CDP este habilitado se puede usar `show cdp`

![[Pasted image 20250114062010.png]]

Los valores sobre los que se hace referencia se pueden cambiar de la siguiente forma:

![[Pasted image 20250114062058.png]]

En caso de que se quiera **deshabilitar** CDP de forma global, se puede usar el comando `no cdp run`. Para deshabilitar CDP por interface, se puede usar `no cdp enable` en el _interface config mode_. 

![[Pasted image 20250114062349.png]]

> Si se deshabilita CDP de forma global, CDP no va a estar activo en ninguna de las interface. Independientemente de que luego se configuren estas como `[no] cdp enable`

Tambien se encuentra el comando `show cdp traffic` que muestra estadisticas sobre la cantidad de mensajes CDP que se envian y reciben. 

![[Pasted image 20250114062730.png]]

## CDP commands summary

**CDP `show` commands**

![[Pasted image 20250114062814.png]]

**CDP configuration commands**

![[Pasted image 20250114062838.png]]

