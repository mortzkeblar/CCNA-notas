Como su nombre indica, es un tipo de [[NAT]] el cual realiza un mapping one-to-one de un _inside local (private) address_ a un _inside global (public) address_.

![[Pasted image 20250214044243.png]]

La limitación de este tipo de NAT es que necesitamos una IP pública o externa por cada IP privada que necesitemos hacer NAT. Puede resultar útil si necesitamos conectar un host particular a internet. 
![[Pasted image 20250214044800.png]]
El comando para realizar el mapping one-to-one es `ip nat inside source static <inside-local> <inside-global>`, antes a eso se necesita especificar las interfaces _inside_ y _outside_ como muestra la imagen.

![[Pasted image 20250214045226.png]]

- El `inside` keyboard indica al router que debe realizar las traducciones de las [[IP address]] originadas desde la _inside network_, a medida que vayan reenviandose hacia el _outside netwok_.
- El `source` keyboard le indica que debe traducir las _source IP address_ provenientes desde los hosts en el _inside network_
	- Es importante aclarar que el reverso tambien es valido cuando se genera la respuesta desde el _outside host_, en este caso el router tomara la respuesta y realizara la traducción para que pueda llegar al _internal host_

Para poder verificar el funcionamiento se puede usar `show ip nat translations` que nos muestra la tabla de traducciones del router.

![[Pasted image 20250227063627.png]]

