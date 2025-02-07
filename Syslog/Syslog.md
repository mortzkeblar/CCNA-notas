_Syslog_ es un estándar de protocolos para _message logging_ basadas en niveles de severidad, proporciana mecanismos para recolectar información sobre los dispositivos y la salud de los mismos. 

Por defecto, Cisco IOS muestra los _syslog messages_ a los usuarios conectados via _console line_ y _VTY lines_. 
- En console lines, esto puede ser controlado con `logging console`, para desactivar se puede usar `no logging console`.
- En VTY lines ([[SSH]] / Telnet), esto es aplicado con el comando `logging monitor`, para deshabilitar se puede usar `no logging monitor`
	- Los mensajes mostrados por pantalla no estan habilitados por defecto en las VTY lines, para eso se debe usar `terminal monitor` en el _privileged EXEC mode_ que lo habilita para una sesión particular, y dura lo que dura esa sesión 

![[Pasted image 20250202222258.png]]
### `logging synchronous` command 
Se puede usar el comando `logging synchronous` para volver a imprimir el texto que se estaba escribiendo en consola, cuando este llega a ser cortado por algún mensaje syslog que pueda aparecer por pantalla. 

![[Pasted image 20250202223100.png]]
### Storing logs
En Cisco IOS, existen dos formas de almacenar los mensajes syslogs historicos: _device's memory_ y _external Syslog server_.

Por defecto, los dispositivos Cisco almacenan los mensajes syslog en la RAM, llamada tambien _logging buffer_. Esto puede ser controlado con el comando `logging buffered [bytes]`, habilitado por defecto. 

Para ver los logs almacenados en el buffer se puede usar `show logging`, al principio de la salida muestra tambien información sobre la configuración de Syslog. Se puede observar los mensajes para cada tipo de configuración, ya sea `console`, `monitor` o `buffer`. 

![[Pasted image 20250203001918.png]]

#### Logging to a Syslog Server 
Además tambien se puede configurar un servidor Syslog, para centralizar los mensajes de logs. Resulta util en redes complejas y simplifica la revisión de logs en lugar de iniciar sesión en cada dispositivo particular. 

Para esto se usa el comando `logging trap` (habiltado por defecto), para especificar el servidor Syslog se debe usar `logging [host] <ip-address>` o `logging <ip-address>` que tiene el mismo efecto. Una vez configurado, este comienza a enviar mensajes syslog al puerto [[UDP]] 514 del servidor configurado. 

> Existe una versión segura de Syslog llamada _Syslog over TLS_, que usa [[TCP]] port 6514


## Syslog message format 

![[Pasted image 20250203005544.png]]
### Sequence numbers and timestamps
Tanto el campo _sequence_ como _timestamp_ son opcionales, por lo general los valores por defecto son que sequence esta deshabilitado y timestamp habilitado. 

Se puede habilitar los sequence numbers con `service sequence-numbers` en el _global config mode_, una vez habilitado este se aplica para los mensajes futuros luego de aplicarse el comando. 

Los timestamps se puede manejar con `service timestamps log`, este tiene una serie de opciones a configurar:
- `service timestamps log datetime` - indica con la fecha y hora cuando ocurre evento
	- Por defecto, los mensajes syslog estan marcados sobre UTC, lo cual puede ser util cuando se tiene dispositivos en distintas zonas horarias. Para modificar esto se puede usar `service timestamps datetime localtime`
	- Tambien se puede agregar los milisegundos a la salida de hora con `service timestamps log datetime msec`
- `service timestamps log uptime` - indica el evento ocurrido con el tiempo de actividad (uptime) del dispositivo, quiere decir desde que se inicio 

### Syslog severity levels 
Los mensajes syslog son categorizados por su severidad. Existen 8 niveles de seguridad, un nivel mientras más bajo sea su valor implica mayor severidad del evento. 

Las _keyboards_ de niveles usadas en Cisco pueden variar del standard syslog (RFC 5424). 

![[Pasted image 20250203015443.png]]
- Los primeros dos niveles de severidad (0 - 1) estan reservados para eventos particulares serios: crashes system, fallas criticas de hardware y otros eventos que requieren atención inmediata. 
- Los niveles (2 - 4) son descritos por el RFC 5424 como condiciones criticas, error o warning, son eventos que deben ser tomados en cuenta pero no necesariamente requieren atención inmediata 
- Los niveles (5 - 6) son usadas normalmente para mensajes de estados operativos rutinarios o notificaciones de cambios menores que no afectan la operatividad ni la seguridad
- El nivel 7 es un nivel único, esta reservado para los mensajes que se generan cuando se tiene activado la depuración mediante el comando `debug`

Es posible controlar de forma independiente los tipos de eventos que se quiere registrar en cada destino (console, VTY, buffer o syslog server).
- Estos pueden ser activado mediante `logging console`, `logging monitor`, `logging buffered` y `logging trap` respectivamente. 
	- El valor registrado por defecto para todos los destinos es `debugging`(excepto para el syslog server que por defecto esta habilitado para registrar hasta los eventos de severidad 6 `informational`), lo que quiere decir que todos los eventos (desde 0 hasta 7) van a ser registrados. 

![[Pasted image 20250203115624.png]]
Estos valores pueden ser modificados para indicar hasta que nivel de severidad se quiere registrar. Por ejemplo para el console line se puede usar `logging console 4` (o su equivalente especificando la keyword `logging console warnings`), para habilitar el registro de la consola desde el nivel 4 y menores (0 - 4).

![[Pasted image 20250203120329.png]]
### `debug` command 
El comando `debug` permite obtener información en real-time sobre las operaciones que ocurren en un dispositivo Cisco, estos puede incluir la actividad de la red, procesos internos, etc. Esto resulta particularmente util cuando se necesita hacer troubleshooting.

Este comando tiene varios keywords que se puede usar dependiente el tipo de información que se desea obtener, por ejemplo `debug spanning-tree events` o `debug ip ospf adj`.

![[Pasted image 20250203134618.png]]

Es importante considerar que este comando hace uso de CPU y memoria intensivos, por lo que su uso debe ser limitado. 



