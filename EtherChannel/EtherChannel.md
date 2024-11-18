> **Bandwidth** es el número total de bits que pueden ser transferidos sobre una conexión por segundo (4x enlaces de un 1Gbps significa 4Gbps de bandwidth). Es importante diferenciar el concepto de bandwidth del de _speed_.

_EtherChannel_ es una tecnologia que permite agrupar varios enlaces fisicos como un único enlace lógico (lo que permite superar la barrera que pueda establecer [[Project/Networking/CCNA-notas/Spanning Tree Protocol/(LEGACY) STP/STP|STP]] para prevenir los layer 2 loops), esto permite mayor disponibilidad del bandwidth además de generar resiliencia y facilidad en la administración. 

![[Pasted image 20241117105419.png]]

> Las interfaces logicas creadas mediante EtherChannel son llamadas _Port Channels_ en Cisco IOS. En el contexto del CCNA, se considera ambos terminos equivalentes. 

Los frames reenviados por el puerto etherchannel, se distribuyen sobre todos los enlaces fisicos, esto es llamado _load balancing_ o _load distribution_.

![[Pasted image 20241117110144.png]]
El puerto al ser tratado como un solo enlace logico, no existen riesgos de que pueda generar un layer 2 loop, como se ve en la imagen anterior. Esto se puede comprobar con `show spanning-tree`.

![[Pasted image 20241117110540.png]]

## EtherChannel configuration 
Se tienen dos caminos para poder configurar etherchannel: 
- *Dynamic EtherChannel* - este modo hace que los [[switch]]es usen un protocolo para negociar entre ellos la formación de un etherchannel link 
- *Static EtherChannel* - este modo fuerza al switch a formar un etherchannel con los puertos especificados, sin ningun tipo de negociación con el vecino 

### Dynamic EtherChannel 
_Dynamic EtherChannel_ se apoya en protocolos de negociación, el switch usa el protocolo para enviar mensajes entre ellos y hacer el proceso de negociación par formar finalmente el etherchannel link. 

> Al tratar de configurar etherchannel se pueden producir layer 2 loops. Por lo que se recomienda usar mayormente dynamic etherchannel sobre static. 

Cisco [[switch]]es soportan dos protocolos de negociación dynamic etherchannel: 
- _Port Aggregation Protocol (PAgP)_ - propietario de Cisco 
- _Link Aggregation Control Protocol (LACP)_ - definido en el IEEE 802.3ad

#### PAgP configuration
Cisco PAgP permite configurar hasta 8 enlaces fisicos como un unico enlace etherchannel. 

Para configurar un enlace PAgP se usa `channel-group <group-number> mode {disarable | auto}` dentro de interface config.
- `group-number` identifica el etherchannel en el [[switch]], permite saber que puertos pertenecen a determinado etherchannel 

Los modos `disarable` y `auto` son los que determinan si se forma o no el enlace etherchannel. 

![[Pasted image 20241117114750.png]]

