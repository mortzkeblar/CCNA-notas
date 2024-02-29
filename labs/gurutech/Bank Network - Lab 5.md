
![](_anexos_/Untitled%20Diagram.drawio.png)

Radeon Company Ltd. is una compañia de US que brinda servicios bancarios y de aseguramiento. Esta intentando expandir sus servicios a través del continente africano abriendo su primer sucursal en Nairobi, Kenya. La compañia adquirio un edificio de cuatro pisos para operar dentro de la capital Keniana, cada piso cuenta con varios departamentos. Los detalles de los requerimientos se encuentran descritos en este paper. 

**First Floor**

| No. | Departaments   | No. of PC |
| --- | -------------- | --------- |
| 1   | Managemenet    | 20        |
| 2   | Research       | 20        |
| 3   | Human Resource | 20        |
**Second Floor**

| No. | Departaments | No. of PC |
| --- | ------------ | --------- |
| 1   | Marketing    | 20        |
| 2   | Accounting   | 20        |
| 3   | Finance      | 20        |

**Third Floor**

| No. | Departaments        | No. of PC |
| --- | ------------------- | --------- |
| 1   | Logistics and store | 20        |
| 2   | Customer care       | 20        |
| 3   | Guest area          | 20        |

**Fourth Floor**

| No. | Departaments   | No. of PC                                        |
| --- | -------------- | ------------------------------------------------ |
| 1   | Administration | 20                                               |
| 2   | ICT            | 20                                               |
| 3   | Server Room    | 2 Admins PC and 3 servers (DHCP, HTTP and Email) |

## Requiriments
1. Use un software de modelación de red para visualizar la topologia de red
2. Puede usar un software de emulación para implementar la siguiente topologia
	- Debe haber un router para cada piso (floor). Los routers debe esta conectados a los switches en ese piso
	- Use [OSPF](../../Routing%20Protocols/OSPF.md) como protocolo de enrutamiento
	- Cada departamento requiere tener una red inalambrica para sus usarios
	- Se espera que cada departamento (excepto el server room) tenga al menos 60 usuarios totales
	- Se espera que todos los hosts en la red deben obtener IP de forma automatica, con un servidor DHCP ubicado en el server room
	- Los dispositivos de todos los departamentos deben comunicarse entre si
	- Crear HTTP y E-mail servers 
	- Configurar SSH en todos los routers para el acceso remoto
3. Use el diseño de jerarquia de red con redundancia incluida
	- Core, distribution and access layers
4. Configurar cuestiones basicas en los dispositivos, a saber 
	- Hostnames
	- Line console and VTY passwords
	- Banner messages
	- Disable domain IP lookup
5. Cada departamento debe tener un VLAN diferente
	- Cada VLAN debe tener una diferente subred
6. Planeamiento de las IP address 
	- Nos asignaron la red 192.168.10.0 como la dirección base para esta red
	- Hacer [subnetting](../NetWarriors/subnetting.md) en base a la cantidad de hosts que necesita 
	- Identificar la mascara de subred, direcciones IP usable y dirección broadcast para cada subnet
7. Configuración port-security
	-  Usar sticky command para obtener la MAC address 
	- Usar shutdown en case de un Violation Mode 
8. Documentar el diseño del proyecto y la implementación


## resolution 

Habilitamos todos los puertos en los routers Cisco, en los routers Mikrotik no es necesario. 

``` bash
conf t
int range g0/0-4 
no shutdown 
```

Antes de configurar los dispositivos, debemos asignar las VLAN en sus respectivos departamentos. Yo voy a usar este orden.
- First floor
	- MANAGENT - VLAN 10
	- HUMAN-FINANCE - VLAN 20
	- HUMAN FINANCE - VLAN 30
- Second floor
	- MARKETING - VLAN 40
	- FINANCE - VLAN 50
	- FINANCE - VLAN 60
- Third floor 
	- LOGISTICS AND STORE - VLAN 70
	- CUSTOMER CARE - VLAN 80
	- GUEST AREA - VLAN 90
- Fourth floor
	- ADMINISTRATION - VLAN 100
	- ICT - VLAN 110
	- SERVER ROOM - VLAN 120

Configuración de cuestiones basicas (VTY, banner, etc)

``` bash
en
conf t
hostname F2-SW-FINANCE
banner motd #This Floor2 FINANCE  switch#
enable secret cisco
no ip domain lookup 

crypto key generate rsa modulus 1024
username ccna secret ccna
line vty 0 4
transport input ssh
login local 
exit
ip ssh version 2
ip ssh time-out 15
ip ssh authentication-retries 3
```

Configuramos las VLAN y las puertos de seguridad en los enlaces de acceso.
``` bash
conf t
int range g1/0-3,g2/0-3
switchport mode access 
switchport access vlan 20

switchport port-security mac-address sticky 
switchport port-security violation shutdown
```