Consiste en el _physical access control_, es decir el control del acceso por medios fisicos a un dispositivo. En esta entrada, se habla sobre el control de acceso a la CLI de los dispositivos Cisco IOS. 

Cuando nos conectamos al _console port_ de un equipo para acceder a la CLI, se dice que se esta conectando a la **_console line_**. 

> Un _line_ es un camino logíco en el dispositivo que nos permite conectarnos y administrar el dispositivo vía CLI. 

 El _console line_ es usado cuando nos conectamos a la CLI a través del _console port_. Para proteger el console port, se debe configurar el console line para realizar las restricciones en el acceso, para ingresar a la configuración se usa `line con 0`

![[Pasted image 20250118224847.png]]
El número 0 indica que es el primer console line, en realidad en los Cisco IOS solo existe un único console line por lo que ese es el único valor valido.

> Los VTY (usadas para la administración remota como [[SSH]]) pueden tener varias entradas para el acceso a la CLI, generalmente suelen ser 16 VTY lines.

## Line password 
Para asegurar el console line, se puede configurar una contraseña para el acceso dentro del _access line config mode_. 
- El comando `login` indica a los usuarios que deben ingresar la contraseña para acceder al console line, sin esto la contraseña no surge efecto
- Luego de acceder con la contraseña, este te deja el _user EXEC mode_
	- Al escalar al _privileged EXEC mode_, tambien se puede proteger con una contraseña mediante el comando `enable secret <password> `, esto se hablo en [[Cisco IOS CLI#Password-protecting privileged EXEC mode]]

![[Pasted image 20250118225217.png]]

## User account authentication 
El comando para crear un usuario es `username <user> secret <password>` (la contraseña es encriptada como un hash, gracias al `secret` command)
- Es importante agregar el `login local` luego de configurar una cuenta local para que este pueda surtir efecto 

![[Pasted image 20250118230624.png]]

> Es importante aclarar que `login` y `login local` no pueden ser configuradas de forma simultanea, si se configurar los dos, el último en ser configurar _overwrite_ al anterior.

### Cisco IOS privilege levels 
Cisco IOS usa _privilege levels_ para el acceso del usuario a los comandos de la CLI. Estos consisten en 16-levels, desde 0 hasta 15. 
- Por defecto, los usuarios que se conectan a través del console line o VTY tienen un _privilege level_ de 1, tambien conocido como _user EXEC mode_ que permite un acceso _read-only_ limitado 
- Cuando se usar `enable` en el user EXEC mode, al usuario se le asigna un _privilege level_ de 15 conocido tambien como _privileged EXEC mode_, que te permite tener control total 

Se puede especificar el _privilege level_ de un usuario al configurarlo con `username <user> privilege <level> secret <secret>`


