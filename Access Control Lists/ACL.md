**Access Control List (ACL)** son un lista ordenada de reglas que filtran paquetes recibidos o reenviados desde una interface.

Cada ACL consiste en una serie de reglas llamadas _Access Control Entries (ACEs)_. Cada ACE consiste en:
- Especificar una condición de match (para una IP o rango de IPs)
- Especifica una acción (_permit_ o _deny_)
	- Los paquetes que son _permitted_ son reenviados 
	- Los paquetes que son _denied_ son descartados 

![[Pasted image 20241220090512.png]]

> La acción final _deny_ no se define como un ACE sino que se encuentra implicito.

- Los paquetes son evaluados por cada ACE en forma ordenada, desde arriba hacia abajo.
- Si un paquete hace match con una condición de ACE, el router toma la acción especificada y no sigue realizando la evaluación con los ACE restantes. 

Es importante que el orden de las reglas ACE sean de la regla más especifica a la menos especifica, para evitar que una regla sea ignorada debido a que una regla menos especifica este antes por lo que los paquetes no se evalúan en la regla que definimos (también llamado _shadowed rule_).

## Implicit `deny`
Un router Cisco por defecto, reenvia cualquier paquete solamente con que se encuentre un ruta en la [[Routing Table]].

Cuando se usa ACL en un router, esto cambia. En este caso, cualquier paquetes que no este especificada como _permit_ en un ACL es denegado por defecto. 

Al final de cada ACL, se aplica una regla oculta que denega cualquier paquete que no haga match con ninguna de las ACE especificada en una ACL, esta regla es llamada _implicit deny_. Esto asegura que solo se reenvie el trafico explicitamente permitido en la ACL y todo lo demás sea descartado de forma automatica. 

## Applying ACLs 
Los ACL son aplicados sobre las interfaces, estas pueden ser aplicadas sobre el trafico _inbound_ o _outbound_ (o ambos).
- _Inbound ACL_ - evalua los paquetes que ingresan por una interface sobre la que se aplica el ACL. 
	- Si el ACL permite el paquete, el router continua procesando ese paquete 
	- SI el ACL deniega el paquete, este se descarta 
- _Outbound ACL_ - evalua los paquetes que salen por una interface. Cuando el router indica que el paquete debe ser reenviado, este primero es evaluado por la ACL de salida.  
	- Si el ACL permite el paquete, el router reenvia el mismo 
	- Si el ACL deniega el paquete, este se descarta 

![[Pasted image 20241220093718.png]]

