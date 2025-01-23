---
tags:
  - CCNA
---
**Access Control List (ACL)** son un lista ordenada de reglas que filtran paquetes recibidos o reenviados desde una interface.

Cada ACL consiste en una serie de reglas llamadas _Access Control Entries (ACEs)_. Cada ACE consiste en:
- Especificar una condición de match (para una IP o rango de IPs)
- Especifica una acción (_permit_ o _deny_)
	- Los paquetes que son _permitted_ son reenviados 
	- Los paquetes que son _denied_ son descartados 

![[Pasted image 20241220090512.png]]

> La acción final _deny_ no se define como un ACE sino que se encuentra implicito como el ultimo ACE, en todos los ACLs configurados.

- Los paquetes son evaluados por cada ACE en forma ordenada, desde arriba hacia abajo.
- Si un paquete hace match con una condición de ACE, el router toma la acción especificada y no sigue realizando la evaluación con los ACE restantes. 

Es importante que el orden de las reglas ACE sean de la regla más especifica a la menos especifica, para evitar que una regla sea ignorada debido a que una regla menos especifica este antes por lo que los paquetes no se evalúan en la regla que definimos (también llamado _shadowed rule_).

### Implicit `deny`
Un router Cisco por defecto, reenvia cualquier paquete solamente con que se encuentre un ruta en la [[Routing Table]].

Cuando se usa ACL en un router, esto cambia. En este caso, cualquier paquetes que no este especificada como _permit_ en un ACL es denegado por defecto. 

Al final de cada ACL, se aplica una regla oculta que denega cualquier paquete que no haga match con ninguna de las ACE especificada en una ACL, esta regla es llamada _implicit deny_. Esto asegura que solo se reenvie el trafico explicitamente permitido en la ACL y todo lo demás sea descartado de forma automatica. 

### Applying ACLs 
Los ACL son aplicados sobre las interfaces, estas pueden ser aplicadas sobre el trafico _inbound_ o _outbound_ (o ambos).
- _Inbound ACL_ - evalua los paquetes que ingresan por una interface sobre la que se aplica el ACL. 
	- Si el ACL permite el paquete, el router continua procesando ese paquete 
	- SI el ACL deniega el paquete, este se descarta 
- _Outbound ACL_ - evalua los paquetes que salen por una interface. Cuando el router indica que el paquete debe ser reenviado, este primero es evaluado por la ACL de salida.  
	- Si el ACL permite el paquete, el router reenvia el mismo 
	- Si el ACL deniega el paquete, este se descarta 

![[Pasted image 20241220093718.png]]

> Los standard ACLs usualmente se aplican en el _outbound_ de la interface conectado a la [[LAN]] de destino que se desea proteger, esto asegura el filtrado de paquetes destinados a esa LAN. 
> Los ACL de tipo _inbound_ suelen ser aplicados para que algún trafico no deseado no pueda ingresar al router y por ende no acceder a ninguna de las redes conectadas al router (i.e interface WAN)

### ACL types 
Los ACL puede ser categorizados según dos caracteristicas:
- _Matching parameters_  
	- Los _standard ACLs_, realizan el match de paquetes basados en un unico parametro: **source [[IP address]]**. 
	- _Extended ACLs_, permiten filtrado más granular al agregar más parametros a considerar como pueden ser: source/destination [[IP address]], source/destination port numbers, etc. 
- _Identification method_ - Numbered ACLs, named ACLs 
	- _Numbered ACLs_, son identificados por un número 
	- _Named ACLs_, son identificados por un nombres

![[Pasted image 20241224234023.png]]


## Configuring Standard ACLs 
![[Pasted image 20241224235739.png]]
Configuración y aplicación de un _standard numbered_ y un _named_ ACL para el control de trafico de la red. 

### Numbered ACLs 
Estos ACLs son identificados con números, existen rangos reservados según el tipo de ACL que se quiera usar: 
- _Standard IP ACLs_ - `[1 - 99]` y `[1300 - 1999]`
- _Extended IP ACLs_ - `[100 - 199]` y `[2000 - 2699]`

El comando principal para configurar un ACE en un _standard numbered ACL_ es `access-list <acl-number> <{ permit | deny }> <source-ip> <wildcard-mask>`. 

![[Pasted image 20241225000841.png]]
- Es importante aclarar que para un standard ACL se debe encontrar en el rango especificado anteriormente.
- Tambien se puede ver que se usa los [[Dynamic Routing#Wildcard Masks]] para especificar un ACE 

Para verificar la configuración se puede usar `show access-list` o `show ip access-list` para las ACLs basados en IP. 

![[Pasted image 20241225001236.png]]
Se puede ver que cada ACE es asociada a un número de secuencia, el primer ACE (el más especifico) comienza en $10$ y a medida que se le agregan más ACEs este incrementa $+10$. Esto permite tener especio para agregar nuevos ACEs en el medio de los ACEs ya configurados, aunque esto solo es posible en el _named ACL_ config mode. 

Luego de creado el ACL, este debe ser aplicado sobre una interface. Para esto se usa el comando `ip access-group <acl-number> { int | out }`

![[Pasted image 20241225001839.png]]

Como resultado de la configuración anterior, vemos que los paquetes son filtrados cuando R2 los tiene que reenviar por `G0/0`, controlando el acceso hacia el Server LAN A.

![[Pasted image 20241225151030.png]]
> La regla general es aplicar el standard ACL lo más cerca posible del destino. Debido a que este tipo de ACL realiza el filtrado según la IP de origen, aplicar el ACL lo más cerca del destino asegura que solo bloqueemos el trafico destinado a esa LAN y no otro trafico que pueda ser legitimo. 

### Named ACLs 
Como su nombre indica, los _named ACLs_ son identificados por medio de nombres. Además tambien difieren en los modos de configuración en el router. 
- Un standard ACL se configura y aplica enteramente en el _global_ config mode 
- Un named ACL son credas en _global_ config mode, luego cada ACE es configurada en un config mode propio, llamado _standard named ACL config mode_.
	- Ingresar el standard named ACL config mode: `ip access-list standard <acl-name>`
	- Configurar ACEs: `<seq-num> {permit | deny} <source-ip> <wildcard-mask>`

> De forma opcional, se puede configurar el _sequence number_ para determinar donde es insertado un ACE. Por defecto el primer ACE tiene el valor de $10$ e incrementa en $+10$ a medida que se agregan más ACE. 

La sintaxis de configuración de un es más flexible cuando se hace match para un unico [[IP address]], por ejemplo estos tres comando tiene el mismo efecto:
- `deny 8.8.8.8` 
- `deny 8.8.8.8 0.0.0.0`
- `deny host 8.8.8.8`

![[Pasted image 20241225173534.png]]
La keyboard `permit any` tiene el mismo efecto que `permit 0.0.0.0 255.255.255.255`.

![[Pasted image 20241225173802.png]]

> También se puede configurar _named ACLs_ especificando un número, sin embargo cuando se realizar la busqueda en `running-config` aparece en la forma de un _numbered ACL_.
> ![[Pasted image 20241225174338.png]]