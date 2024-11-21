---
tags:
  - VLAN
  - CCNA
date created: Sunday, November 10th 2024, 12:56:46 am
date modified: Wednesday, November 20th 2024, 11:33:50 pm
---
Este metodo  de [[Inter-VLAN routing]] hace uso de [[switch]]es multilayer los cuales poseen un router integrado, estos permiten crear interfaces virtuales llamadas _Switch Virtual Interfaces (SVIs)_, cada SVI es un interface asociada con un [[VLAN]] y una IP address que los hosts finales usan como [[Default gateway]]. 

![[Pasted image 20241109214514.png|500]]

En un [[switch]] multilayer, es necesario habilitar IP routing con el comando `ip routing`. Sin esto, el switch no puede hacer forwarding de paquetes entre subredes/VLANs. 

``` bash
# SW1 config 
ip routing 
interface vlan 10 
ip address 172.16.1.1 255.255.255.192 
interface vlan 20
ip address 172.168.1.65 255.255.255.192 
interface vlan 30
ip address 172.16.1.129 255.255.255.192 
```

Este al igual que un router, posee una [[Routing Table]] en la que se agregan [[Local route]] y [[Connected route]]s. Por lo cual no es necesario agregar static routes de forma manual.

![[Pasted image 20241109235619.png|500]]

Para que una SVI pueda funcionar, necesita cumplir una serie de requisitos. El estado se puede comprobar con `show ip interface brief`.
- La [[VLAN]] asociada al SVI debe estar creada en el switch (con `vlan <vlan-id>`)
- El switch debe cumplir al menos alguno de los requisitos 
	- Un access port asociado con la VLAN (con estado UP/UP)
	- Un trunk port en la que ese VLAN este permitida (con estado UP/UP)
- La VLAN debe estar habilitada (no debe tener aplicado `shutdown`)
- El SVI debe estar habilitado (no debe tener aplicado `shutdown`)

![[Pasted image 20241109235640.png]]

### Routed ports for external connectivity 
Para proporcionar conectividad con redes externas en un [[switch]] multilayer, se hace uso de un _routed port_. Un routed port es un puerto fisico que se configura para funcionar como un puerto de un router. 

![[Pasted image 20241110003602.png|500]]

Para configurar un router port, se debe usar el comando `no switchport` en la interface a configurar. 

``` bash
interface g0/1
no switchport 
ip addresss 172.16.1.193 255.255.255.252

# configure a default static route 
ip route 0.0.0.0 0.0.0.0 172.16.1.194
```

![[Pasted image 20241110003928.png|500]]