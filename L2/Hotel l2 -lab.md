---
tags:
  - lab
---
Este laboratorio parteneca al canal [Gurutech](https://www.youtube.com/@gurutechnetworks), playlist _Real-World Networking Projects_, [Cap3](https://www.youtube.com/watch?v=RwFJTJTe-OM&list=PLvUOx2WG6R7PlKlERb5zceXxHfC4P7gJn&index=15).

``` bash
# comandos de ayuda
Switch$ show interface <INTERFACE> switchport 
Switch$ show vlan brief

Router$ show ip ospf neighbor 
```

![](_anexos_/gtech%20lab%203.png)

## objetives
Se requiere implementar una red moderna para un Hotel. El hotel consta de tres pisos:
- El primer piso contiene tres departamentos
	- Reception
	- Store 
	- Logistics
- El segundo piso contiene tres departamentos 
	- Finance
	- HR
	- Sales/Marketing
- El tercer piso contiene un departamento 
	- IT/admin

Además, se deben seguir estas consideraciones a la hora de la implementación de la red.
1. Tiene que haber tres routers que conecten cada piso (todos ubicados en el server room en el departamento de IT).
2. Todos los routers deben estar conectados entre si usando cables DCE serial.
3. La red entre los routers debe ser: `10.10.10.0/30`, `10.10.10.4/30`, `10.10.10.8/30`.
4. Se espera que cada piso tenga un switch (ubicado en su respectivo piso).
5. Se espera que cada piso tenga redes Wifi conectados a dispositivos inalambricos.
6. Se espera que cada departamento tenga un impresora
7. Se espera que cada departamento tenga su propia VLAN con los siguientes detalles:
	1. Primer piso
		1. Reception - VLAN80 - red: `192.168.8.0/24`
		2. Store - VLAN70 - red: `192.168.7.0/24`
		3. Logistics - VLAN60 - red: `192.168.6.0/24`
	2. Segundo piso 
		1. Finance - VLAN50, red: `192.168.5.0/24`
		2. HR - VLAN40, red: `192.168.4.0/24`
		3. Sales - VLAN30, red: `192.168.3.0/24`
	3. Tercer pisos 
		1. Admin - VLAN20, red: `192.168.2.0/24`
		2. IT - VLAN10, red: `192.168.1.0/24`

8. Usar [OSPF](../Routing%20Protocols/OSPF.md) como protocolo de enrutamiento para anunciar rutas. 
9. Todos los dispositivos en la red deberian poder obtener una IP dinamicamente con sus respectivos routers configurados como DHCP server. 
10. Todos los dispositivos de la red deberian poder comunicarse entre si.
11. Configurar SSH en todos los routers para administración remota.
12. En el departamento de IT, añadir una PC llamada `Test-PC` al puerto `G0/1` y usarlo como dispositivo donde hacer los tests de conección remota.
13. Configurar port security para el IT-dept [switch](../switch.md), para permitir el acceso solo a Test-PC en Gi0/1 (usar sticky method para obtener la MAC address con modo de violación: shutdown).


## resolution

Procedemos a configurar las VLAN en access mode en los switches para los distintos departamentos. 

``` bash
# SWF1 

en 
conf t

vlan 80
name RECEPTION
exit
vlan 70
name STORE 
exit
vlan 60
name LOGISTICS
exit

int range g1/0-3
switchport mode access
switchport access vlan 80

int range g2/0-3
switchport mode access
switchport access vlan 70

int range g3/0-3
switchport mode access
switchport access vlan 60

# SWF2

en 
conf t

vlan 50
name FINANCE
exit
vlan 40
name HR 
exit
vlan 30
name SALES
exit

int range g1/0-3
switchport mode access
switchport access vlan 50

int range g2/0-3
switchport mode access
switchport access vlan 40

int range g3/0-3
switchport mode access
switchport access vlan 30

# SWF3

en 
conf t

vlan 20
name ADMIN
exit
vlan 10
name IT
exit

int range g1/0-3
switchport mode access
switchport access vlan 20

int range g2/0-3
switchport mode access
switchport access vlan 10
```

Configuramos los trunks ports en cada uno de los switches.

``` bash
# SWF1

int g0/0
switchport trunk encapsulation dot1q
switchport mode trunk
switchport trunk allowed vlan 80,70,60

# SWF2

int g0/0
switchport trunk encapsulation dot1q
switchport mode trunk
switchport trunk allowed vlan 50,40,30

# SWF3

int g0/0
switchport trunk encapsulation dot1q
switchport mode trunk
switchport trunk allowed vlan 20,10
```

Asignamos las IP a los routers en la red `10`. 

``` bash
# RF1

conf t
int g0/0
ip addr 10.10.10.6 255.255.255.252
no sh

int g0/1
ip addr 10.10.10.10 255.255.255.252 
no sh

# RF2

conf t
int g0/0
ip addr 10.10.10.1 255.255.255.252 
no sh 

int g0/1
ip addr 10.10.10.9 255.255.255.252 
no sh

# RF3

conf t
int g0/0
ip addr 10.10.10.2 255.255.255.252 
no sh

int g0/1
ip addr 10.10.10.5 255.255.255.252 
no sh
```

Ahora configuramos en los routers los enlaces troncales, las interfaces virtuales y la asignación de IPs.

``` bash
# RF1

int g0/2
no shut 

int g0/2.80
encapsulation dot1q 80
ip address 192.168.8.1 255.255.255.0
int g0/2.70
encapsulation dot1q 70
ip address 192.168.7.1 255.255.255.0
int g0/2.60
encapsulation dot1q 60 
ip address 192.168.6.1 255.255.255.0

# RF2

int g0/2
no shut 

int g0/2.50
encapsulation dot1q 50
ip address 192.168.5.1 255.255.255.0
int g0/2.40
encapsulation dot1q 40
ip address 192.168.4.1 255.255.255.0
int g0/2.30
encapsulation dot1q 30 
ip address 192.168.3.1 255.255.255.0


# RF3

int g0/2
no shut 

int g0/2.20
encapsulation dot1q 20
ip address 192.168.2.1 255.255.255.0
int g0/2.10
encapsulation dot1q 10
ip address 192.168.1.1 255.255.255.0
```

Configuramos [DHCP](../DHCP/DHCP.md) en cada uno de los routers.

``` bash
# RF1

service dhcp
ip dhcp pool DHCP_RECEPTION
network 192.168.8.0 255.255.255.0
default-router 192.168.8.1 
dns-server 192.168.8.1
exit

ip dhcp pool DHCP_STORE
network 192.168.7.0 255.255.255.0
default-router 192.168.7.1 
dns-server 192.168.7.1
exit

ip dhcp pool LOGISTICS_DHCP
network 192.168.6.0 255.255.255.0
default-router 192.168.6.1 
dns-server 192.168.6.1
exit

# RF2

service dhcp
ip dhcp pool DHCP_FINANCE
network 192.168.5.0 255.255.255.0
default-router 192.168.5.1 
dns-server 192.168.5.1
exit

ip dhcp pool DHCP_HR
network 192.168.4.0 255.255.255.0
default-router 192.168.4.1 
dns-server 192.168.4.1
exit

ip dhcp pool DHCP_SALES
network 192.168.3.0 255.255.255.0
default-router 192.168.3.1 
dns-server 192.168.3.1
exit

# RF3

service dhcp
ip dhcp pool DHCP_ADMIN
network 192.168.2.0 255.255.255.0
default-router 192.168.2.1 
dns-server 192.168.2.1
exit

ip dhcp pool DHCP_IT
network 192.168.1.0 255.255.255.0
default-router 192.168.1.1 
dns-server 192.168.1.1
exit
```

Ahora configuramos [OSPF](../Routing%20Protocols/OSPF.md) en los routers bajo el ID 10.

``` bash
# RF1

router ospf 10
network 10.10.10.4 255.255.255.252 area 0
network 10.10.10.8 255.255.255.252 area 0
network 192.168.8.0 255.255.255.0 area 0
network 192.168.7.0 255.255.255.0 area 0
network 192.168.6.0 255.255.255.0 area 0

# RF2

router ospf 10
network 10.10.10.0 255.255.255.252 area 0
network 10.10.10.8 255.255.255.252 area 0
network 192.168.5.0 255.255.255.0 area 0
network 192.168.4.0 255.255.255.0 area 0
network 192.168.3.0 255.255.255.0 area 0

# RF2

router ospf 10
network 10.10.10.0 255.255.255.252 area 0
network 10.10.10.4 255.255.255.252 area 0
network 192.168.2.0 255.255.255.0 area 0
network 192.168.1.0 255.255.255.0 area 0
```

Habilitamos SSH.

``` bash
# RF1

hostname RF1
ip domain-name gtech
crypto key generate rsa modulus 1024
username gtech secret gtech
line vty 0 4
transport input ssh
login local 
exit
ip ssh version 2
ip ssh time-out 15
ip ssh authentication-retries 3

# RF2

hostname RF2
ip domain-name gtech2
crypto key generate rsa modulus 1024
username gtech2 secret gtech2
line vty 0 4
transport input ssh
login local 
exit
ip ssh version 2
ip ssh time-out 15
ip ssh authentication-retries 3

# RF3

hostname RF3
ip domain-name gtech3
crypto key generate rsa modulus 1024
username gtech3 secret gtech3
line vty 0 4
transport input ssh
login local 
exit
ip ssh version 2
ip ssh time-out 15
ip ssh authentication-retries 3

```

Al conectarnos por SSH, no va a permitir que podemos entrar a la configuración global hasta que asignemos una contraseña para el mode EXEC. 

``` bash
enable secret gtech
```


#SSH Ahora nos conectamos desde Test-PC a los routers. 

``` bash
ssh -o KexAlgorithms=+diffie-hellman-group1-sha1 -o     HostKeyAlgorithms=+ssh-rsa <USERNAME>@<IP ADDRESS>
```

Realizamos el ultimo requerimiento en SWF3, ya que Test-PC se encuentra conectados a ese switch. 

``` bash
en
conf t
int g2/1
switchport port-security
switchport port-security maximum 1
switchport port-security mac-address sticky 
switchport port-security violation shutdown
```