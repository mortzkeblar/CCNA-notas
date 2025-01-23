---
tags:
  - CCNA
---
**Network Time Protocol (NTP)** es un protocolo que nos permite sincronizar los dispositivos en base a una fuente de tiempo comun. Es importante mantener esto ya que muchos protocolos como TLS u OSPF tienen mecanismos que necesitan tener precisión con las fechas. 

### Setting the date and time 
Para ver la hora de un dispositivo Cisco se puede usar `show clock` 

![[Pasted image 20250114080030.png]]

`Hardware calendar` significa que tiene reloj de bateria que mantiene el tiempo incluso cuando el dispositivos esta apagado. Se puede consultar con `show calendar`.

Si la hora tiene un (`*`) por delante, quiere decir que la hora no es _authoritative_, por lo que no se considera exacta. 

#### Setting the clock and calendar 
Se puede usar `clock set <hh:mm:ss> <month> <day> <year>` en _privileged EXEC mode_ para establecer un reloj por software.

![[Pasted image 20250114080629.png]]
Para modificar el reloj `calendar` se usa el mismo comando pero cambiando `clock` por `calendar`.

Tambien existen comandos para sincronizar la misma hora desde un reloj a otro:
- Sincronizar el `calendar` con la hora de relog - `clock update-calendar`
- Sincronizar el reloj de tiempo con la hora de `calendar` - `clock read-calendar`

#### Configuring the time zone 

El time zone por defecto de los dispositivos Cisco es **Coordinated Universal Time (UTC)**, para cambiar la zona horario se puede usar `clock timezone <name> <hours-offset> <minutes-offset>` en _global config mode_.
- `hours-offset` y `minutes-offset` son parametros que indican el desplazamiento con respecto a UTC. Para Buenos Aires el desplazamiento es de `-3`

![[Pasted image 20250114082855.png]]

#### Configuring daylight saving time/summer time 
El horario de verano o _daylight saving time (DST)_ se puede configurar con `clock summer-time <name> recurring <start-date-time> <end-date-time>` en _global config mode

![[Pasted image 20250114083430.png]]

## How NTP works 
> NTP tiene una exactitud de menos de 1 segundo para un server ubicado dentro de la misma LAN y entre 10 - 100 milisegundos para servers ubicados en internet 

NTP usa un modelo jerarquico. 
- En la cima se encuentra los _references clocks_, dispositivos de mucha precisión como relojes atómicos o GPS. 
- La _distancia_ de un NTP server con respecto a un reference clock es llamado _stratum_, un server con un _stratum_ más bajo significa que es más cerca del _reference clock_
	- La distancia no hace referencia a las distancia fisica sino a la distancia dentro de la jerarquia NTP
	- El _stratum_ más alto posible es 15, un _stratum_ de 16 o más significa que no es confiable
- Los clientes preferiran conectarse a un NTP server con una distancia más baja 

![[Pasted image 20250115002615.png]]
- Un dispositivo puede ser un server y un cliente NTP a la vez (excepto los server de _stratum_ 1, un server _stratum_ 2 es cliente del server _stratum_ 1
- Un NTP server de stratum 1 es tambien llamado _primary server_
- Un NTP server que consigue su tiempo de otro server es llamado _secondary server_
- Un dispositivo puede ser un cliente de multiples NTP servers, lo que es util para agregar redundancia 
	- El cliente usa algoritmos para determinar cual server tiene el mejor tiempo más preciso para ser usado 
- Los NTP server puede formar con otros servers un _peer relationship_ que les permite intercambiar información ente ellos, esto es una relación bidireccional. 
	- Los servers puede usar esta información proporcionada por su pares para mejorar la precisión general de sus calculos de tiempo
	- Tambien agrega redundancia ya que le permite sincronizarse con sus pares en caso de que un server pierda conectividad con su server primario, pueda mantener la sincronización gracias a sus pares 

### Configuring NTP 
En los routers y [[switch]]es Cisco, se puede user NTP como server, client y peer. Además de poder aplicar autenticación.
#### NTP client mode 

Se puede especificar usar un NTP server con `ntp server <ip-address> [prefer]`, el argumento `prefer` se usa para favorecer la elección de ese server sobre otros configurados, aunque no es una garantia de elección ya que se toman en cuenta otros parametros como el _stratum_ del server. 

![[Pasted image 20250115005431.png]]
Se puede confirmar la configuración usando `show ntp associations`, tambien se puede usar `show ntp status`

![[Pasted image 20250115005603.png]]
- `* sys.peer` indica el NTP server con el que se mantiene la sincronización 
- `+ candidate` indica que el NTP server se considera como candidato para ser una fuente de tiempo, pero que no esta siendo usado actualmente 
- `address` indica la IP address del NTP server 
- `ref clock` indica la fuente de tiempo del servidor, en este caso `GOOG` es un reloj de referencia que pertenece a Google
- `st` indica el _stratum_ del servidor NTP 

NTP solo sincroniza el _software clock_ por defecto, no el _hardware calendar_. En este caso se debe usar `ntp update-calendar` en _global config mode_ para que NTP actualice de forma periodica el _hardware calendar_

#### NTP server mode 
Cuando se configura un router / [[switch]] como un NTP client, esta ya puede ser usada por otros dispositivos como un NTP server. 

![[Pasted image 20250115012917.png]]
> El comando `ntp server` convierte a un dispositivo en modo client / server

Una practica recomendada para usar un NTP server en un dispositivo Cisco es configurar una interface _loopback_ y que la [[IP address]] de esa interface se use para el NTP server. El comando `ntp source <interface>` indica que el equipo debe generar mensajes NTP desde la interface configurada.

> Los clientes NTP envian mensajes NTP request al puerto UDP 23 de los NTP servers

![[Pasted image 20250115013228.png]]
![[Pasted image 20250115013507.png]]

Tambien se puede usar `ntp master` para convertir a un dispositivo en un _primary NTP server_ usando su propio reloj interno como _reference clock_. Esto permite configurar un NTP server sin necesidad de ser un NTP client de otro server. 

Los comandos `ntp server` y `ntp master` no son excluyentes entre si, se pueden usar ambos para que en caso de el router pierda conectividad con los servidores NTP externos se pueda usar un NTP server local como backup. 

![[Pasted image 20250115014446.png]]
Se puede ver que el servidor local tiene un _stratum_ de 7, este es un valor arbitrario para que tenga menos peso que los servidores NTP externos en caso de elección. Esto puede ser modicado con `ntp master <value-stratum>`

![[Pasted image 20250115014839.png]]

El valor configurado hace referencia al valor que tendrá en la jerarquia cuando se sincronice con el reloj local. Pero no varia en el valor establecido para los servidores externos. 

#### NTP authentication 
Los servidores NTP publicos pueden no ser apropiados en entornos enterprise por motivos de seguridad. En este caso, se puede proteger los NTP clientes mediante autenticación. 

La autenticación en NTP valida que el NTP server sea legitimo y no un atacante que pudo haber hecho _spoofed_ (falsificado) la IP address. 

![[Pasted image 20250115022809.png]]
![[Pasted image 20250115022835.png]]
 - `ntp master` convertimos el dispositivo en un NTP server 
 - `ntp authentication-key <key-number>  md5 <key>` establece la contraseña
	 - `key-number` es un identificador númerico, tiene que hace match en el server y el cliente
	 - `key` es la propia contraseña, tiene que hacer match en el server y el cliente
	- `md5` es un algoritmo de hashing, no se considera seguro por lo que se debe elegir otras alternativas de ser posible 

**Configuración de lado del cliente**
![[Pasted image 20250115024443.png]]
> Se puede usar la misma secuencia para configurar NTP peers reemplazando `ntp server <ip-address> key <key-number>` por `ntp peers <ip-address> key <key-number>`

