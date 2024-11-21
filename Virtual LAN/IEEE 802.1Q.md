---
tags:
  - CCNA
  - VLAN
date created: Saturday, November 9th 2024, 9:02:47 am
date modified: Wednesday, November 20th 2024, 11:33:50 pm
---
El protocolo IEEE 802.1.q, es usado para _taggear_ los frames en los puertos troncales o trunk ports. Es un campo de 4 bytes que se añade entre los campos source y ethertype del [[Ethernet Header (and Trailer)]]. 

![[Pasted image 20241109095009.png]]
- _Tag Protocol Identifier (TPID)_ - campo de 16 bits de tamaño, siempre contiene el valor `0x8100`. Este valor le permite al [[switch]] identificar que el frame esta taggeado usando 802.1q. 
- _Tag Control Information (TCI)_ 
	- _Priority Code Point (PCP)_ - campo de 3 bits que se usa para marcar los frames como _higher_ o _lower_ según su prioridad, esto se usa para QoS. 
	- _Drop Elegible Indicator (DEI)_ - campo de 1 bit, puede ser usada para indicar los frames que pueden ser droppeados si la red se encuentra congestionada
	- _VLAN Identifier (VID)_ - es un campo de 12 bits que indica en que [[VLAN]] se encuentra el frame. El cual a su vez da el total de VLANs permitidas ya que $2^{12}=4096$. 


### Configuring 802.1Q Trunk Port

``` bash
# configuring trunk port in GigabitEthernet0/0
interface g0/0
switchport trunk encapsulation dot1q 
switchport mode trunk 
```

Para verificar los trunk ports se usa el comando `show interfaces trunk`. 

![[Pasted image 20241109104029.png|500]]
> Una vez configurada una interface como trunk, esta ya no aparece en la salida al ejecutar `show vlan brief`.

- _Vlans allowed on trunk_ - estas son todas las VLANs permitidas en el puerto troncal, por defecto se permite el rango `1 - 4094`.
- _Vlans allowed and active in management domain_ - este contiene la lista de VLANs permitidas y que existen en el switch. Es importante que las VLANs sean creadas en el [[switch]] ya que sino no se permite el trafico de estas, independiente de que las VLANs permitidas este como _all_.
	- _Management domain_, es una referencia al protocolo VTP. 

Es una practica comun permitir solo las [[VLAN]]s necesarias dentro de un trunk port. Esto además limita el tamaño de los broadcast domain, ya solo permites el trafico broadcast y unknown unicast de las VLANs permitidas. El comando se configuración es `switchport trunk allowed vlan <option>`. 

![[Pasted image 20241109174426.png|400]]


