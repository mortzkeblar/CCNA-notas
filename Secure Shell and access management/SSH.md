---
tags:
  - CCNA
---
**Secure Shell (SSH)** es un protocolo que permite la conexión remota a dispositivos a través de la red. A diferencia de [[Remote Management#Configuring Telnet]], SSH es un protocolo seguro ya que encripta las comunicaciones entre el cliente y servidor. 

La configuración de SSH consiste en los siguientes pasos:
- Generar _cryptographic RSA keys_
- Habilitar las conexiones SSH en las VTY lines 

### Generating RSA keys 
SSH requiere un par de _cryptographics keys_ para la encriptación y desencriptación de mensajes, estas keys usan _RSA algorithms_.
![[Pasted image 20250120011823.png]]

Antes de poder generar un key, es necesario contar con un _domain name_ ya que los par de keys usan el FQDN como los nombres del par de keys. 
- Una vez configurado el domain name, se puede proceder con la key mediante `crypto key generate rsa`
- También se puede configurar un RSA key sin un domain name usando `crypto key generate rsa label <name>`, para esta opción no es necesaria un FQDN y permite configurar multiples keys pairs para distintos propositos

![[Pasted image 20250120012041.png]]

Al generar el RSA key, se le consultara un serie de preguntas para poder completar la configuración:
- Tamaño de los _key modulus_ (determina la fuerza de la encriptación, un valor mayor es más seguro). 
	- Cisco recomienda usar valores mayores a 2048 bits 
	- Tambien se puede especificar el tamaño en el comando `crypto key generate rsa modulus <bits>`

Para confirmar la configuración se puede usar `show ip ssh`
![[Pasted image 20250121003631.png]]

> SSH en los dispositivos Cisco tiene dos versiones mayores:
> - SSH version 1 (SSHv1)
> - SSH version 2 (SSHv2)
> Cuando un dispositivo indica que la versión de SSH es 1.99, quiere decir que acepta conexiones tanto de tipo v1 como de v2, por seguridad se recomienda usar solo v2.


### Configuring SSH 

![[Pasted image 20250121003854.png|600]]

![[Pasted image 20250121003933.png]]

- Las consideraciones como `enable secret` y `acl` se explicaron en [[Remote Management#Configuring Telnet]]
- `ip ssh version 2` fuerza a que las conexiones solo sean permitidas usando SSHv2
	- El RSA key pair's _modulus size_ debe ser de al menos 768 bits para poder usar SSHv2


