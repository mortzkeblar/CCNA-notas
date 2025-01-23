---
tags:
  - CCNA
---
Para la administración remota de los dispositivos Cisco, se tiene principalmente dos opciones a considerar:
- Telnet 
- [[SSH]] 

### Management IP address 
- Tanto Telnet como [[SSH]] funcionan con un modelo cliente - servidor, para que se puedan comunicar se necesita que ambos tengan [[IP address]] configuradas.
	- En los routers, se puede conectar a cualquier IP que se encuentra configurada en una interface
	- En los switches se trata de la siguiente forma:
		- En los L3 [[switch]]es, se puede conectarse tanto las IPs configuradas en los _switch virtual interfaces (SVIs)_ como en los _routed ports_
			- Los SVIs en este caso pueden enrutar paquetes
		- En los L2 [[switch]]es, se puede configurar _SVIs_ para poder acceder de forma remota 
			- Los SVIs no pueden enrutar paquetes, pero si puede recibir y enviar

![[Pasted image 20250119005223.png]]

**Configuring an SVI and [[Default gateway]] on a L2 switch**
![[Pasted image 20250119005527.png]]

Es importante que se configure `ip default-gateway <ip-address>` para que el switch puede saber donde enviar los paquetes en destinos remotos. 


![[Pasted image 20250119010000.png]]

#### Management IP address best practices 
- Un management [[IP address]] de un router se suele configurar sobre un [[OSPF#Loopback interfaces]], esto garantiza  estabilidad e independencia del estado de un puerto particular 
- Un management [[IP address]] de un [[switch]] se suele configurar en un SVI de una VLAN especifica destinada a ser un _management VLAN_
	- Un management VLAN es una VLAN que no transporta trafico regular y que suele estar destinado solamente para el administración remota 
	- Por motivos de seguridad, el trafico de administración se debe mantener aislado de cualquier otro trafico en la red 

## Configuring Telnet 
Telnet tambien llamado _Teletype Network_, desarrolado en 1969. En un protocolo de comunicación simple pero insegura ya que envia los datos en _cleartext_, lo que puede ser interceptado si alguien hace _sniffing_ del trafico transportado. 

![[Pasted image 20250119010831.png]]

- Al configurar Telnet, es requisito que antes se deba configurar una contraseña para el _privileged EXEC mode_
	- Sin esto, no se nos permitirá pasar al modo _privileged EXEC mode_
- Es un buena practica configurar un ACL, para limitar los hosts que pueden conectarse al dispositivo, luego de crear el ACL se aplica sobre la VTY (no la interface) con `access-class <acl> in`
- Se configura el rango de _console line_ para el acceso a través de la VTY con `line vty <first-line> <last-line>`, Generalmente se configuran 16 VTYs (0 - 15)
- `login local` le indica que para la conexión se requiere la autenticación de un usuario local 
- `exec-timeout <minutes>` es un comando opcional  que indica el tiempo  de espera de inactividad, por defecto es un valor de 10 minutos
- `transport input <protocol>` especifica mediante que protocolo nos conectamos a las VTY lines. Entre las opciones disponibles estan:
	- `transport input telnet`
	- `transport input ssh`
	- `transport input telnet ssh`
	- `transport input all`
	- `transport input none`

Para verificar la configuración se puede usar `show running-config`, se puede ver que existe una división en las VTY lines de (0-4) y (5-15), esto es para mantener la compatibilidad con los dispositivo más antiguos que solo permiten tener 5 VTYs (0-4)

![[Pasted image 20250119021142.png]]

