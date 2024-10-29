---
tags:
  - lab
  - RIP
  - VLAN
  - CCNA
---

Temas a tratar:
- [RIPv2](RIPv2.md) 
- [VLAN](../../VLAN/VLAN.md) 

La universidad Albion es un centro muy grande donde se ubican dos campus a 20 millas de distancia. Los estudiantes de la universidad y el staff estan distribuidos en cuatro facultades, estos incluyen a la facultad de Ciencias de la Salud, Negocios, Ingenieria/Computación y Arte/Diseño. Cada miembro del staff tiene una PC y los estudiantes tienen acceso a las PCs en los laboratorios. 

## requirements
1. Crear una topologia de red con los siguientes parametros a seguir:
	1. Campus Principal
		1. Edificio A - aca se encuentra el staff administrativo en los siguientes departamentos: Gestión, HR y Finanzas. Las PC del admin staff estan distribuidos en la oficinas del edificio y se espera que ellos compartan algunos equipos de red (Pista: se espera el uso de VLANs aqui). La escuela de negocios se encuentra en este edificio tambien.
		2. Edificio B - Facultad de Ingenieria y Computación y la Facultad de Artes y Diseño.
		3. Edificio C - laboratorios de estudiantes y el departamento de IT. El departamento de IT alberga los servidores web de la universidad y otros servicios.
			1. También hay un servicio de mail albergado en la nube. 
	2. Campus Menor
		1. Facultad de Ciencias de la Salud (el staff y los labs de estudiantes se encuentran en pisos distintos).
2. Se espera que configures los dispositivos centrales y algunos host finales para proporcionar conectividad end-to-end y acceso a los servidores internos y al servidor externo.
	1. Cada departamento/facultad deberia tener su propia red IP y separada del resto. 
	2. Los switches deben ser configurados con las VLAN correspondientes y configuraciones de seguridad.
	3. [RIPv2](RIPv2.md) sera usado para proporcionar enrutamiento entre los routers en la red interna, además de enrutamiento estatico para el servidor externo. 
	4. Se espera que los dispositivos en los edificios obtengan IPs de forma dinamica desde un router que actue como servidor [DHCP](../../DHCP/DHCP.md). 


## resolution

Para planear la arquitectura nos basamos en el modelo de jerarquias de Cisco, algo que es abordado en [LAN y WAN - Jerarquias](../NetWarriors/LAN%20y%20WAN%20-%20Jerarquias.md). 

Tanto las VLANs como las redes internas de cada departamento van a estar conformados de la siguiente manera.

main campus 
- Edificio A
	- management: VLAN 10, 192.168.1.0/24
	- HR: VLAN 20, 192.168.2.0/24
	- finance: VLAN 30, 192.168.3.0/24
	- business: VLAN 40, 192.168.4.0/24
- Edificio B
	- enginnering: VLAN 50, 192.168.5.0/24
	- art and design: VLAN 60, 192.168.6.0/24
- Edificio C
	- labs: VLAN 70, 192.168.7.0/24
	- it: VLAN 80, 192.168.8.0/24

minor campus
- staff: VLAN 90, 192.168.9.0/24
- st-labs: VLAN 100, 192.168.10.0/24

### configurations 

Comenzamos levantando las interfaces en los routers

``` bash
# RMAIN

hostname RMAIN
conf t
int range g0/0-1
no shut 
int g0/3
no shut

# RCLOUD
hostname RCLOUD 
conf t
int range 0/1-2
no shut

# RMINOR

hostname RMINOR 
conf t
int range 0/0-1
no shut
```

Ahora configuramos los switches de acceso que estan conectados a los hosts. 

``` bash
# example for SW-ADMIN
en
conf t
int range g0/0-3,g1/0-3,g2/0-3,g3/0-3
switchport mode access 
switchport access vlan 90
```

Tambien configuramos los switches L3 que esta conectadas con los otros switches en access mode. 

``` bash

# SWL3-main-campus

en
conf t

int g1/0
switchport mode access 
switchport access vlan 20
int g1/1
switchport mode access 
switchport access vlan 30
int g1/2
switchport mode access 
switchport access vlan 40
int g1/3
switchport mode access 
switchport access vlan 10

int g2/0
switchport mode access 
switchport access vlan 50
int g2/1
switchport mode access 
switchport access vlan 60

int g3/0
switchport mode access 
switchport access vlan 70
int g3/1
switchport mode access 
switchport access vlan 80

# SWL3-minor-campus

en
conf t

int g2/0
switchport mode access 
switchport access vlan 90
int g1/0
switchport mode access 
switchport access vlan 100

```

Configuración de trunk ports en switches L3.

``` bash
# SWL3-main-campus 
int g0/0
switchport trunk encapsulation dot1q
switchport mode trunk 

# SWL3-minor-campus
int g0/0
switchport trunk encapsulation dot1q
switchport mode trunk 
```

Configuración del routing interVLAN en RMAIN y RMINOR.

``` bash
# rmain-campus 

int g0/1
no shut

int g0/1.10
encapsulation dot1q 10
ip address 192.168.1.1 255.255.255.0 
int g0/1.20
encapsulation dot1q 20
ip address 192.168.2.1 255.255.255.0 
int g0/1.30
encapsulation dot1q 30
ip address 192.168.3.1 255.255.255.0 
int g0/1.40
encapsulation dot1q 40
ip address 192.168.4.1 255.255.255.0 
int g0/1.50
encapsulation dot1q 50
ip address 192.168.5.1 255.255.255.0 
int g0/1.60
encapsulation dot1q 60
ip address 192.168.6.1 255.255.255.0 
int g0/1.70
encapsulation dot1q 70
ip address 192.168.7.1 255.255.255.0 
int g0/1.80
encapsulation dot1q 80
ip address 192.168.8.1 255.255.255.0 
do sh ip int br 

# rminor-campus 

int g0/1
no shut

int g0/1.90
encapsulation dot1q 90
ip address 192.168.9.1 255.255.255.0 
int g0/1.100
encapsulation dot1q 100
ip address 192.168.10.1 255.255.255.0 
do sh ip int br
```

Configuración de IP estaticas entre los routers en la red `10`.

``` bash
# rmain-campus 
int g0/0
ip address 10.10.10.1 255.255.255.252
no sh
int g0/3 
ip address 10.10.10.5 255.255.255.252 
no sh
exit

# rminor-campus 
int g0/0
ip address 10.10.10.2 255.255.255.252
no sh
exit

# rcloud 
int g0/1
ip address 10.10.10.6 255.255.255.252 
no sh
int g0/2
ip address 20.0.0.1 255.255.255.252 
no sh
exit
```

Configuración de RMAIN y RMINOR como servidor [DHCP](../../DHCP/DHCP.md).

``` bash
# rmain 

service dhcp
ip dhcp pool DHCP_ADMIN
network 192.168.1.0 255.255.255.0
default-router 192.168.1.1 
dns-server 1.1.1.1
exit
ip dhcp pool DHCP_HR
network 192.168.2.0 255.255.255.0
default-router 192.168.2.1 
dns-server 1.1.1.1
exit
ip dhcp pool DHCP_FINANCE
network 192.168.3.0 255.255.255.0
default-router 192.168.3.1 
dns-server 1.1.1.1
exit
ip dhcp pool DHCP_BUSINESS
network 192.168.4.0 255.255.255.0
default-router 192.168.4.1 
dns-server 1.1.1.1
exit
ip dhcp pool DHCP_ENGINEERING
network 192.168.5.0 255.255.255.0
default-router 192.168.5.1 
dns-server 1.1.1.1
exit
ip dhcp pool DHCP_ART_DESIGN
network 192.168.6.0 255.255.255.0
default-router 192.168.6.1 
dns-server 1.1.1.1
exit
ip dhcp pool DHCP_LABS
network 192.168.7.0 255.255.255.0
default-router 192.168.7.1 
dns-server 1.1.1.1
exit
ip dhcp pool DHCP_IT
network 192.168.8.0 255.255.255.0
default-router 192.168.8.1 
dns-server 1.1.1.1
exit

# rminor 

service dhcp
ip dhcp pool DHCP_STAFF
network 192.168.9.0 255.255.255.0
default-router 192.168.9.1 
dns-server 1.1.1.1
exit
ip dhcp pool DHCP_ST_LABS
network 192.168.10.0 255.255.255.0
default-router 192.168.10.1 
dns-server 1.1.1.1
exit
```

#TODO Asignar una ruta estatica al mail server 20.0.0.2 

Configuración de [RIPv2](RIPv2.md) en los RMAIN y RMINOR.

``` bash
# rmain 

router rip 
versi 2
network 10.10.10.0 
network 10.10.10.4 
network 192.168.1.0
network 192.168.2.0
network 192.168.3.0
network 192.168.4.0
network 192.168.5.0
network 192.168.6.0
network 192.168.7.0
network 192.168.8.0
exit

# rminor 

router rip
vers 2
network 10.10.10.0 
network 192.168.9.0
network 192.168.10.0
exit

# rcloud 
router rip
ver 2 
network 10.10.10.4 
network 20.0.0.0
exit
```