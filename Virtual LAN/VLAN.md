---
tags:
  - VLAN
  - CCNA
date created: Saturday, October 19th 2024, 11:08:14 pm
date modified: Wednesday, November 20th 2024, 11:33:50 pm
---


Las _Virtual Local Area Network_ (o VLAN) son agrupaciones lógicas de dispositivos en un mismo dominio broadcast (ver [[domain in network]]). 

Esto es bastante util para _dividir_ el switch en diferentes segmentos LAN, porque con VLAN podemos crear subredes con sus propios dominios broadcast y definir que que dispositivos queremos que comuniquen a través de esa subred. 

![[Pasted image 20241109083311.png|500]]

Considerar que por defecto un switch solo tiene un dominio broadcast para enviar todo el trafico de la red y ese es el número `1`, también llamado [VLAN Native](VLAN%20Native.md).
- Las VLAN aumentan la cantidad de dominios broadcast al tiempo que reducen su tamaño.
- Las VLAN reducen los riesgos de seguridad al reducir la cantidad de hosts que reciben copias de las tramas que inundan los switches.

## Configuring VLANs 
Para ver las lista de VLANs existentes en un [[switch]], se puede usar `show vlan brief`.

![[Pasted image 20241109083941.png|500]]

- VLANs 1002 al 1005, son usadas por FDDI y Token Ring (legacy data link layer technology)

Para crear una VLAN se puede usar `vlan <vlan-id>` siendo `<vlan-id>` un valor entre el rango $[1, 1001] \cup[1006, 4094]$. De forma opcional se puede configurar una nombre a una VLAN con el comando `name <vlan-name>`.

![[Pasted image 20241109084650.png|500]]

### Access ports 
Un puerto de acceso es un puerto que se asigna a una unica VLAN, esto implica que solo hace forwarding y flood a los puertos que se encuentren dentro de la misma VLAN. 

``` bash
# configures G0/0-3 as access ports in VLAN 10 
interface range g0/0-3 
switchport access mode 
switchport access vlan 10
```
> Si se usa el comando `switchport access vlan <vlan-id>` de una VLAN que no existe, esta se crea automaticamente. 

Es importante destacar que si bien comunmente se habla de que _un host X esta configurada en la VLAN Y_. En realidad los hosts finales desconocen el proceso de VLANs, por lo cual esto es transparente a ellos. 

> Otro nombre comun para un puerto de acceso es _untagged ports_. 

### Trunks ports 
Estos puertos a diferencia de los access ports, puede reenviar el trafico de multiples VLANs a través de un unico puerto. El protocolo más usado para configurar trunk ports es [[IEEE 802.1Q]]. 

![[Pasted image 20241109090211.png|500]]
Para esto, el [[switch]] añade una etiqueta o tag antes de enviar el trafico por un trunk port.
- El switch que recibe el frame revisa el tag y asigna el frame a la VLAN especificada en el tag
	- Si el destino se encuentra en un VLAN diferente al que se encuentra en el tag, el paquete no va a ser reenviado.  

> Otro nombre comun para un puerto troncal es _tagged ports_.





### References
- https://www.ccnablog.com/vlans-part-i/
- https://study-ccna.com/what-is-a-vlan/