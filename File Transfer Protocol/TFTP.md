> Tanto FTP como SFTP en el contexto del CCNA son importantes para la transferencia de archivos que permite cosas como el upgrading del SO de los equipos Cisco 

_Trivial File Transfer Protocol (TFTP)_ es un protocolo de red con un modelo cliente - servidor usada para la transferencia de archivos a través de la red. 

TFTP no usa ningun tipo de autenticación, ni encriptación, no es necesario tener un usuario y contraseña para poder conectarse al servidor TFTP.
- Estas caracteristicas hacen que TFTP sea inseguro de usar en redes externas como internet, pero puede ser practico en redes más seguras como un [[LAN]] interna debido a la simplicidad 

Un cliente TFTP inicia la comunicación enviando ya sea un _write request_ o _read request_
- _read request_ es una solicitud para descargar un archivo desde el server 
- _write request_ es una solicitud para cargar un archivo al server 

Un TFTP server escucha los _requests_ en UDP port 69, el protocolo funcionan con una comunicación _lock-step_ en la que el cliente y el servidor envian mensajes en forma alternada a la espera de una respuesta.
- Se hace uso del _acknowledgment_. Cada vez que el server (download) o el cliente (upload) envia un bloque de información, este no va a enviar el siguiente bloque hasta que haya recibido un ACK.
- Despues de que cualquier de los dispositivos envie un mensaje, se establece un _timer_
	- Si el timer luego de enviar un mensaje expira, se realiza una _retransmission_ de ese mensaje
		- Cuando un dispositivo recibe un mensaje de _transmission_ entiendo que el mensaje enviado falló en llegar a destino, por lo que reenvia nuevamente ese mensaje 


![[Pasted image 20250204114136.png]]

#### TFTP Transfer ID 
Anteriormente se dijo que los TFTP servers escuchan los requests en UDP port 69, pero en realidad esto solo es valido para los mensajes iniciales _read requests_ y _write requests_.

Luego de los _requests_ iniciales, el server asigna un nuevo puerto dentro de los _ephemeral range_ (49152 - 65535) para poder identificar cada sesión, esto es llamado _Transfer ID (TID)_

## Transferring files with TFTP 
#### Cisco IOS File Sysem 
_Cisco IOS file system_, es un conjunto de varios file systems, que estan creadas para administrar diferentes sectores del storage como la flash memory, NVRAM, etc. Estos diversos file systems se pueden ver mediante `show file systems`

![[Pasted image 20250204121328.png]]

Entre los file systems se pueden encontrar:
- _Opaque_ - logical file system usada para funciones internas (i.e `system`)
- _NVRAM_ - usada para la NVRAM del dispositivo (donde se almacena el `startup-config`)
- _Disk_ - almacenamiento fisico del dispositivo 
- _Network_ - usado para el acceso a file systems externos a través de la red (i.e. TFTP server)

**_Opaque file system_**
`system:` es _opaque_ logical file system donde se almacena el archivo `running-config`. Para verificar esto entre otros se puede usar el comando `dir`

![[Pasted image 20250205011744.png]]

Para ver el contenido de un archivo se usa el comando `more`, para ver el archivo de configuración que generalmente se ve con `show running-config`, se puede usar `more system:running-config` en este caso especificando la ruta del archivo. 

**_NVRAM file system_**
El NVRAM file system type solo tiene un file system, `nvram:.`. El cual almacena el archivo `startup-config`, esto se puede comprobar con `dir nvram:` o `more nvram:startup-config`. 

![[Pasted image 20250205012701.png]]
**_Disk file system_**
Este file system es usado por los dispositivos de almacenamiento fisico, uno de ellos es `flash:`. Este es el _default file system_, la ubicación principal donde el sistema operativo busca varios archivos para sus operaciones (a menos que se especifique otro file system).
- Tambien es la ubicación donde se copian los archivos de TFTP o cuando se inserta una memoria flash 

**_Network file system_**
Es usado para el acceso remote a los file system sobre la red, como un TFTP server. Un ejemplo es `tftp:`. 

### `copy` command
Para transferir archivo de o hacia un TFTP server, se puede usar el comando `copy <source> <destination>`.

En el siguiente ejemplo usamos `copy tftp: flash:` para descargar un IOS image de un TFTP server, al usar `tftp:` este nos hara una serie de preguntas para poder ubicar la dirección y la ruta del archivo a descargar. 
- Es importante conocer el nombre del archivo a descargar, ya que TFTP no te permite ver los archivos que hay contenidos en el server
![[Pasted image 20250205104017.png]]

Para saltar los prompts, puede directamente `copy tftp://<server-ip>/<src-filename> flash:/<dst-filename>`

