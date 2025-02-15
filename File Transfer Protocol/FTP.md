_File Transfer Protocol (FTP)_ es un protocolo de transferencia de archivos estándarizado en 1971, es una versión más robusta de [[TFTP]]. Entre algunas de sus caracteristicas:
- Autenticación con username/password 
- No hay encriptación de datos (para esto se puede usar FTP Secure (FTPS) )


En caso de ser necesario la encriptación, se pueden usar las siguientes alternativas 
- _File Transfer Protocol Secure (FTPS)_ - usa TLS para asegurar la comunicación cliente - servidor 
- _SSH File Transfer Protocol (SFTP)_ - una extensión de SSH que permite realizar la transferencia de archivos encriptados (a pesar del nombre, no tiene relacion con FTP)

FTP usa [[TCP]] como protocolo [[Layer 4]], esto significa que debe haber primero un _establishing [[TCP]]_. Una particularidad es que FTP necesita establecer dos conexiones TCP para poder operar, un _FTP control conection_ y un _FTP data conection_

![[Pasted image 20250205123304.png]]

Luego del establishing [[TCP]] (TCP three-way handshake), el cliente y el server tienen que establecer una **_FTP control connection_** en el TCP port 21 siguiendo lo siguente 


	- El server autenticara al cliente verificando el usuario y contraseña 
	- Si la autenticación es exitosa, ahora el cliente puede enviar _FTP commands_ al server, estos comandos indican al server que es lo que quiere hacer el cliente 

El intercambio de datos ocurre en un segunda conexión establecida llamada **_FTP Data connection_** en el TCP port 20, en la que establece primeramente el _establishing TCP_ para luego pasar al intercambio de datos.

> Una ventaja de tener el _data_ y el _control_ connections separados es que permite soportar multiples transferencia de datos de forma simultanea. Con un único _control connection_ el cliente puede establecer multiples _data connection_ con el server. 


## Transferring files with FTP 
La transferencia de archivos en Cisco IOS con FTP es igual que en [[TFTP#Transferring files with TFTP]], con la diferencia de que necesitamos ingresar un usuario y contraseña para poder autenticarnos, para esto debemos usar `ip ftp username <username>` y `ip ftp passsword <password>` antes de operar con un FTP server.

![[Pasted image 20250207114635.png]]
![[Pasted image 20250207114643.png]]

Para especificar todos los parámetros en un solo comando se puede usar `copy ftp://<username>:<password>@<server-ip> flash:`

## Upgrading Cisco IOS 
Para verificar la versión actual del SO que esta corriendo el dispositivo se puede usar `show version`

![[Pasted image 20250207120324.png]]

Para hacer boot del dispositivo con una imagen diferente del que usa, se puede ejecutar `boot system <image-path>` en el _global config mode_. Se puede verificar la imagen que esta en boot con `show boot`

![[Pasted image 20250207121412.png]]
Para que el cambio tenga efecto, se debe guardar el archivo de configuración y reiniciar con `write` y `reload` respectivamente. 

Para borrar la vieja imagen almacenada se puede usar `delete flash:/<image-ios.bin>`
