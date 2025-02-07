**Simple Network Management Protocol (SNMP)** es un protocolo que sirva para facilitar la administración y monitoreo de la red, permite centralizar el estado de los dispositivos y poder manejar los _trigguers_ alerts que se activan en _events_ especificos. 

## SNMP operations and components
Los dos principales componentes de SNMP son:
- _Network Management Station (NMS)_ - se trata de una plataforma de software que centraliza y monitorea la red mediante SNMP  
- _Managed devices_ - dispositivos administrados y monitoreados por el NMS, cada _managed device_ organiza su propia información dentro de un database llamada _Management Information Database (MIB)_
	- El _MIB_ contiene información sobre el estado del dispositivo en forma de variables (i.e. `GigabitEthernet0/1 status`, temperatura interna, estado del proceso OSPF, etc)

### SNMP operations 
El NMS puede consultar a sus _managed devices_ para obtener el valor de una o más variables. SNMP tiene los siguientes tipos de mensajes:
- _Get_ - permite consultar una o más variables del MIB. Tambien se tiene subcategorias para consultar sobre el MIB 
	- Get 
	- GetNext 
	- GetBulk
- _Response_ - la respuesta del _managed devices_ al mensaje _Get_
- _Set_ - permite modificar el valor de las variables del MIB. Esto permite configurar el dispositivo desde el NMS sin necesidad de acceder de forma manual  
- _Trap_ -  permite a los managed devices notificar inmediatamente al NMS sobre un evento ocurrido, a diferencia de un _Get message_ que tiene que ser enviado de forma periodica por el NMS.
### SNMP components 
Cada dispositivo SNMP, ya sea un NMS o un managed devices ejecuta un software llamado _device SNMP entity_.
- SNMP entity del NMS consiste en el _SNMP application_ y el _SNMP manager_
- SNMP entity del managed device consiste en el _SNMP agent_ y el _Management Information Base (MIB)_

![[Pasted image 20250123004326.png]]

#### SNMP managers and agents 
El SNMP manager se ejecuta sobre el NMS enviando mensajes (Get, Set, etc) al SNMP agent que se ejecuta en cada uno de los managed devices, el agent esta en modo escucho y responde a las peticiones que puede solicitar el SNMP manager.

> SNMP Manager: UDP Port 162 
> SNMP agent: UDP port 161

#### Management Information Base (MIB)
_MIB_ es una database en donde cada managed device organiza la información sobre sí mismo, esta información se almacena en forma de variables que luego son consultadas por el SNMP manager. 

Cada variable recibe un identificador único llamado **_object identifier (OID)_**. Los OIDs se organizan de forma jerarquica dentro de una estructura tree-like. Cada número del OID representa un diferente nivel o rama en el arból. 

![[Pasted image 20250123005906.png]]

Tambien existe OIDs propios definidos por los fabricantes, estos pueden ser o no reconocidos por un software SNMP. A menudo los vendors proporcionan los códigos para interactuar con los dispositivos, que pueden ser exportados al SNMP software para que puedan ser interpretados. 

## SNMP messages 
Como se indico anteriormente, SNMP tiene cuatro tipos de mensajes, que pueden tener a su vez sus propias subdiviciones:

![[Pasted image 20250123010447.png]]

### _Read_ message class 
Los mensajes enviados en el _read_ class son originados por el NMS para obtener información de los managed devices. 

- _Get_ - el mensaje get más simple, el NMS especifica uno o varios OIDs que son enviados y respondidos con sus valores por el managed device 
- _GetNext_ - es usada por el NMS para descubrir OIDs desconocidos, se puede usar para realizar la busqueda de OIDs desconocidos a partir de un la jerarquia de un OID conocido, tambien llamado _walking the tree_.
- _GetBulk_ - fue introducido en SNMPv2, permite al NMS ser más eficiente en la captura de información al permitir especificar un rango completo de OIDs y no OIDs individuales.
	- Este tipo de mensajes tambien permite _walk the tree_ como GetNext, pero de forma más eficiente, los GetNext request son uno a la vez mientras un GetBulk request es varios en una vez. 

### _Write_ message class 
Esta clase solo contiene el tipo de mensaje _Set_, que permite al NMS modificar el valor asociado a un OID particular. Estos pueden ser hostnames, [[IP address]], ACLs, etc. 

Tambien aclarar que existen OIDs que son _read-only_, por lo que su valor no puede ser modificado por el mensaje _set_, en general estos OIDs suelen estar relacionado con información del sistema. 

### _Notification_ message class 
Esta clase incluye dos tipos de mensajes, _trap_ e _inform_.
- _Trap_ - este mensaje se ejecuta desde el managed device cuando ocurre un _event_ particular en el dispositivo, en este caso el managed device envia un _trap_ message al NMS notificandolo sobre el evento. 
	- Independientemente de sí el NMS llegó a recibir o no el mensaje, el managed device considera que el NMS fue notificado. Por esto a trap se le considera _unacknowledged notifications_
- _Inform_ - fue introducido en SNMPv2, cumple la misma función que _Trap_. Con la diferencia de que el NMS es capaz de confirmar el recibimiento (acknowledged) del mensaje con un _response_ message
	- Por esto, a _inform_ se le considera _acknowledged notifications_ 
	- En general, _inform_ message es preferible sobre _trap_ en SNMPv2 y SNMPv3

### _Response_ message class 
Esta clase contiene un solo tipo de mensaje llamado _response_, que ya fue cubierto anteriormente. Este tipo de mensaje puede ser usado tanto por el NMS como por los managed devices.
- Los managed devices envian _response_ message para responder a _Get, GetNext, GetBulk_ y _Set_
- El NMS envia response para responder a los _informs_

## SNMP versions and security 

**SNMP versions**
![[Pasted image 20250123020007.png]]

SNMPv1 se desarrollo en base a los RFCs 1155, 1156 y 1157 en 1990. Se incluyen 5 tipos de mensajes: _Get, GetNext, Set, Trap_ y _Response_. La seguridad se basa en _community strings_, contraseñas usadas para autenticar las operaciones SNMP entre el NMS y los managed devices.

SNMPv2 añade dos tipos de mensajes adicionales: _GetBulk_ e _Inform_. Sin embargo, el standard no incluyo la caracteristica de seguridad _community strings_. Por lo que se desarrollo SNMPv2c o tambien _community-based SNMPv2_.

SNMPv3 no incluye nuevos tipos de mensajes, pero mejora el aspecto de seguridad añadiendo _authentication_ y _encryption_. 

### SNMPv1 and SNMPv2c security 
> En SNMP un _community_ es un grupo de SNMP managers y agents que comparten parametros comunes de autenticación, una contraseña llamada _community-string_

SNMPv1 y SNMPv2 usan un modelo de seguridad _community-based_, tienen dos tipos diferentes de _community_: _read-only (RO)_ y _read-write (RW)_
- _read-only (RO)_ - el NMS que proporcione un RO community string en sus mensajes solo puede leer la información del managed device (dispositivo administrado), puede usar _Get, GetNext y GetBulk_ requests. Pero no puede modificar el dispositivo con mensajes _Set_.
- _read-write (RW)_ - el NMS que proporcione un RW community string en sus mensajes tiene la capacidad de leer y escribir la información del dispositivo administrado.

![[Pasted image 20250123094248.png]]

![[Pasted image 20250123093525.png]]

NOTE: Tambien es posible agregar un ACL al community string para que solo los NMS que estan permitidas por el ACL puedan usar el community string con `snmp-server community <community-string> {RO | RW} <acl>`

Para usar mensajes _Trap_ en los dispositivos administrados Cisco, primeramente deben ser habilitados, para luego especificar el SNMP server donde se deben dirigir las notificaciones. 

![[Pasted image 20250123094724.png]]
- También se puede habilitar los traps para eventos específicos como `snmp-server enable traps cpu`
- El _community-string_ en el `snmp-server host` determina la contraseña que el dispositivo va a usar cuando envie mensajes _trap_ e _inform_ al NMS. No necesariamente tienen que coincidir con `snmp-server community` que se usan desde el NMS para obtener o modificar valores.

### SNMPv3 security 
Es importante recordar que los _community-strings_ son enviados en texto plano sin ningún tipo de encriptación, por lo que los datos de una comunicación SNMP pueden ser interceptados. SNMPv3 proporciona un enfoque más fuerte en la seguridad: 
- _User-based_ - en lugar del modelo de seguridad basado en _community strings_, SNMPv3 permite basar su modelo de seguridad basado en usuarios y los accesos que estos tienen sobre determinado recurso 
- _Message integrity_ - SNMPv3 verifica que el mensaje no haya sido alterado antes de llegar a su destino
- _Authentication_ - la autenticación username/password se asegura mediante algoritmos de hashing para que las credenciales no sean visibles en texto plano
- _Encryption_ - los mensajes SNMPv3 ahora pueden ser encriptadas para que solo el destinatario puede leerlos 

Las opciones de autenticación y encriptación son opcionales y pueden ser adaptadas según se desee, SNMPv3 ofrece tres niveles de seguridad: 
- **_NoAuthNoPriv_** - sin autenticación y sin encriptación 
- **_AuthNoPriv_** - autenticación, pero sin encriptación 
- **_AuthPriv_** - autenticación y encriptación 

![[Pasted image 20250124045655.png]]
Para la configuración de SNMPv3 se debe crear primero un grupo con el nivel de seguridad que va a ser usado (`priv` en este caso es _AuthPriv_). Luego se puede asignar usuarios a ese grupo con el comando `snmp-server user`

![[Pasted image 20250124050647.png]]

