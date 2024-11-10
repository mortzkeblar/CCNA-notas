_[[VLAN]] Trunking Protocol (VTP)_ es un protocolo propietario de Cisco (al igual que [[DTP]]) que permite la administración de las [[VLAN]]s en [[switch]]es Cisco. 

Esta hace uso de la _VLAN database_ la cual se sincroniza en todos los [[switch]]es que tienen una configuración VTP. Esta database almacena información sobre las VLANs que existen en el switch. 

Estos se envian mensajes entre si, con la información de su propia VLAN database y sincronizan sus database de acuerdo a la ultima versión de este. Asegurando que todas las VLANs configuradas existan en todos los switches de la LAN.

## VTP synchronization

![[Pasted image 20241110172455.png]]

> Los mensajes VTP solo se envian a través de trunk ports. 

- VTP revision number - este valor hace tracking de la ultima versión de la VLAN database. Esta va aumentando en cada cambio que se realiza. 
	- Los switches solo sincronizan su información cuando la versión del VTP revision number es mayor al que tienen ellos, ya que se encuentra más actualizado.
- VTP domain - es el grupo de switches dentro de la LAN que comparten el mismo VTP domain name. 
	- Por defecto,  los [[switch]]es no tienen un VTP domain name. Se puede verificar el estado con `show vtp status`. En este estado VTP esta desactivado y no envian mensajes. 
	- Una vez que se configura un VTP domain name con `vtp domain <domain-name>` este comenzara a enviar mensajes DTP. 

![[Pasted image 20241110172953.png|500]]

## VTP modes 

Un [[switch]] puede operar en cuetro modos VTP, las cuales se configuran con `vtp mode <mode>`

![[Pasted image 20241110173700.png]]


## VTP versions 
En general siempre es preferible usar la versión 3 de VTP, ya que trae mejoras significativas entre las que se encuentran el mode _off_. Para cambiar de versión se usa el comando `vtp version <version>`. 

### The primary server
En VTPv3, se puede configurar un servidor primario en la que solo el puede crear/modificar y eliminar [[VLAN]]s. Los demás switches toman el rol de servidor secundario (el cual es equivalente a estar en `client` mode). Para esto se usa el comando `vtp primary` en el [[switch]] asignado. 

![[Pasted image 20241110174838.png]]

> Si se ejecutar `vtp primary` en un switch diferente del primario, este se convierte en el primario y el original pasa a ser un servidor secundario.


### Extended-range VLANs 
El rango de VLANs $[1006, 4094]$ son llamadas _extended-range VLANs_, el rango $[1, 1005]$ se denominan _normal-range VLANs_.

VTPv3 permite la posibilidad de propagar las VLANs que se encuentran dentro del extended-range VLANs. 

## Is VTP dangerous? 
Uno de los grandes peligros al usar VTP, es cuando se conecta un switch a la red y esta resulta que tiene un _VTP revision number_ superior al de los demás [[switch]]es. 

Esto tiene como resultado que ese switch va a sobreescribir las VLAN database de todos los demas switches en la red, algo que probablemente no estuviera previsto. 

![[Pasted image 20241110175641.png]]

Para restablecer el VTP revision number a $0$ se tiene estas opciones:
- Cambiar el VTP domain number, y luego devolver al nombre que uno desea
- Cambiar el VTP mode a transparent o off y luego devolver al mode que uno desea

> Este problema tambien se puede evitar si se usa un servidor VTP primario (solo VTPv3).





