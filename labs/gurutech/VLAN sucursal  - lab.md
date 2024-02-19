---
tags:
  - lab
  - VLAN
---
Este laboratorio parteneca al canal [Gurutech](https://www.youtube.com/@gurutechnetworks), playlist _Real-World Networking Projects_, [Cap2](https://www.youtube.com/watch?v=F_dSpaTMyuA&list=PLvUOx2WG6R7PlKlERb5zceXxHfC4P7gJn&index=16). 

``` bash
# comandos de ayuda
Router$ show ip interface brief

Switch$ show vlan brief 
Switch$ show interfaces trunk
```

![](_anexos_/Screenshot%20from%202024-02-13%2010-16-12.png)
## objetives
La compañia XYZ abre una nueva sucursal en Villa Bonobo y requiere un pasante IT para diseñar la red de esa sucursal. La cual consiste de una pequeña red con los siguientes requerimientos.
- Un router y un switch para ser usados (solo CISCO)
- Se tiene tres departamentos (Admin/IT, Finanzas/HR y CustomerServices/Reception)
- Cada departamento requiere una VLAN diferente
- Los hosts IPv4 requieren obtener una dirección IP automaticamente
- Todos los dispositivos de todos los departamentos deben poder comunicarse

Asumimos que el ISP le provee la IP 192.168.1.0/24.

## resolution

Red a implementar: 

``` bash
Network Base: 192.168.1.0/24

No. subnets: 3
No. of subnets: 2^n -> 2^n=3, n=2 -> 4 (aprox)

Subnet mask: 255.255.255.192
Block size: 64

Network ID: 192.168.1.0 
Broadcast ID: 192.168.1.63
Host range: 192.168.1.1 - 192.168.1.62

Network ID: 192.168.1.64
Broadcast ID: 192.168.1.127
Host range: 192.168.1.65 - 192.168.1.126

Network ID: 192.168.1.128
Broadcast ID: 192.168.1.192
Host range: 192.168.1.129 - 192.168.1.191

The network 192.168.1.192 - 192.168.1.255 remains free.

```

Ahora toca configurar el access mode dentro de SW1.

``` bash
en
conf t
int range g0/1-2
switchport mode access
switchport access vlan 10 

int g0/3
switchport mode access
switchport access vlan 20 
int g1/0
switchport mode access
switchport access vlan 20 

int range g1/1-2
switchport mode access
switchport access vlan 30
```

Ahora, siguiendo en SW1. Configuramos `Gi0/0` para que puede ser un enlace troncal con el router. 

``` bash
int g0/0
switchport trunk encapsu dot1q
switchport mode trunk 
switchport trunk allowed vlan 10,20,30
```

Configuramos el router para habilitar el enlace troncal y las interfaces virtuales.

``` bash
conf t
int g0/0
no shut

int g0/0.10
encapsulation dot1q 10
ip address 192.168.1.1 255.255.255.192

int g0/0.20
encapsulation dot1q 20
ip address 192.168.1.65 255.255.255.192

int g0/0.30
encapsulation dot1q 30
ip address 192.168.1.129 255.255.255.192
```

 Procedemos a configurar el servidor [DHCP](../../DHCP/DHCP.md)  dentro del router.

``` bash
service dhcp
ip dhcp pool DHCPVLAN10
network 192.168.1.0 255.255.255.192 
default-router 192.168.1.1
dns-server 192.168.1.1
domain-name admin.com

ip dhcp pool DHCPVLAN20
network 192.168.1.64  255.255.255.192 
default-router 192.168.1.65
dns-server 192.168.1.65
domain-name finance.com

ip dhcp pool DHCPVLAN30
network 192.168.0.128 255.255.255.192 
default-router 192.168.1.129 
dns-server 192.168.1.129
domain-name reception.com
```