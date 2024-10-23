---
tags:
  - CCNA
---
GRE (Generic Routing Encapsulation) es una tecnica de tunelización que permite crear conexiones point-to-point. GRE por si sola no es segura ya que no tiene ningun tipo de encriptación, para agregar seguridad se debe agregar una capa de encriptación como IPSec. 

![[Pasted image 20241009000246.png]]

``` bash
# HQ
HQ(config)#interface fastEthernet 0/0
HQ(config-if)#ip address 192.168.12.1 255.255.255.0
HQ(config-if)#exit
HQ(config)#interface loopback0
HQ(config-if)#ip address 172.16.1.1 255.255.255.0
HQ(config-if)#exit
HQ(config)#ip route 192.168.23.3 255.255.255.255 192.168.12.2

# ISP 
ISP(config)#interface fastEthernet 0/0
ISP(config-if)#ip address 192.168.12.2 255.255.255.0
ISP(config-if)#exit
ISP(config)#interface fastEthernet 1/0
ISP(config-if)#ip address 192.168.23.2 255.255.255.0

# Branch
Branch(config)#interface fastEthernet 0/0
Branch(config-if)#ip address 192.168.23.3 255.255.255.0
Branch(config-if)#exit
Branch(config)#interface loopback 0
Branch(config-if)#ip address 172.16.3.3 255.255.255.0
Branch(config-if)#exit
Branch(config)#ip route 192.168.12.1 255.255.255.255 192.168.23.2
```

Ahora se deben crear rutas estáticas para que tanto branch como HQ se puedan comunicar mediante el ISP router. Las interfaces loopback en este caso actúan como redes LAN para cada router, las cuales se van a comunicar mediante los tuneles GRE. 

``` bash
# HQ 
HQ(config)#interface tunnel 1     
HQ(config-if)#tunnel source fastEthernet 0/0
HQ(config-if)#tunnel destination 192.168.23.3
HQ(config-if)#ip address 192.168.13.1 255.255.255.0

# Branch
Branch(config)#interface tunnel 1
Branch(config-if)#tunnel source fastEthernet 0/0
Branch(config-if)#tunnel destination 192.168.12.1
Branch(config-if)#ip address 192.168.13.3 255.255.255.0
```

En este caso los tuneles ya se encuentran en estado UP y son capaz de comunicarse entre si los routers.

```
HQ#show interfaces tunnel 1
Tunnel1 is up, line protocol is up 
  Hardware is Tunnel
  Internet address is 192.168.13.1/24
  MTU 1514 bytes, BW 9 Kbit, DLY 500000 usec, 
     reliability 255/255, txload 1/255, rxload 1/255
  Encapsulation TUNNEL, loopback not set
  Keepalive not set
  Tunnel source 192.168.12.1 (FastEthernet0/0), destination 192.168.23.3
  Tunnel protocol/transport GRE/IP
```

Para comunicar a las interfaces loopback de cada router, vamos hacerlo anunciando sus redes mediante [[EIGRP]].

``` bash
# HQ
HQ(config)#router eigrp 13
HQ(config-router)#no auto-summary 
HQ(config-router)#network 192.168.13.0
HQ(config-router)#network 172.16.1.0

# Branch 
Branch(config)#router eigrp 13
Branch(config-router)#no auto-summary 
Branch(config-router)#network 192.168.13.0
Branch(config-router)#network 172.16.3.0

```

Con esto las rutas van a estar anunciándose mediante los túneles.
