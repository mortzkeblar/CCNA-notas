---
tags:
  - DHCP
  - lab
  - CCNA
---

``` bash
# Comandos relevantes para depuración
SW1$ show vlan brief 
SW1$ show vlan-switch # igual al comando anterior, esta version suele estar mas en dispositivos viejos
SW1$ show interfaces trunk

## Esto aplica si tienes instalado nmap en tu contenedor o VM
host1$ nmap --script broadcast-dhcp-discover

```

La idea en esta ocación es poder configurar un server DHCP en un switch/router Cisco (en la imagen, SW1-C va actuar como server). Antes de eso vamos a necesitar tener configurados las VLAN y el enrutamiento entre los switches. 

_Ver: [DHCP](../../DHCP/DHCP.md)_ 

![](_anexos_/Screenshot%20from%202024-01-11%2014-32-59.png)

## Configuración previa
En este caso usamos SW1-C como servidor VTP con los demás switches actuando como clientes. Todo esto se ve en [VTP - Lab](VTP%20-%20Lab.md), en todo caso abajo dejo ejemplos de forma basica para esta configuración.

``` bash
# Configuracion de SW1-C en modo server
SW1-C> en
SW1-C$ conf t
SW1-C(config-vlan)$ vlan 10
SW1-C(config-vlan)$ vlan 20
SW1-C(config-vlan)$ vlan 30
SW1-C(config-vlan)$ vlan 40
SW1-C(config-vlan)$ exit

SW1-C(config)$ vtp mode server
SW1-C(config)$ vtp domain cisco.com

SW1-C(config)$ interface range g0/0-3
SW1-C(config-if-range)$ switchport trunk encapsulation dot1q
SW1-C(config-if-range)$ switchport mode trunk
SW1-C(config-if-range)$ switchport trunk allowed vlan 10,20,30,40
SW1-C(config-if-range)$ switchport trunk native vlan 1
```

``` bash
# Configuracion de VTP para SW2 en modo cliente 
## Esta aplicando la conf desde la interface 1 hasta la 3 (la g0/0 queda excluida porque es la que conecta con SW1-C)

SW2> en
SW2$ conf t
SW2(config)$ vtp mode client
SW2(config)$ vtp domain cisco.com
SW2(config)$ int range g0/1-3
SW2(config-if-range)$ switchport mode access
SW2(config-if-range)$ switchport access vlan 40

# Aplicar esto en los switches faltantes: SW3, SW4 y SW5
```

``` bash
# Asignación de IP en las VLAN para enrutamiento 
## Estas configuraciones solo se aplican en el VTP server, osea SW1-C 
## Ejemplo de conf para la VLAN 40

SW1-C$ int vlan 40
SW1-C(config)$ ip address 192.168.40.1 255.255.255.0
SW1-C(config)$ description VLAN40
SW1-C(config)$ no shut
SW1-C(config)$ exit

# Aplicar esto para las VLAN restantes 10, 20 y 30
## La IP puede estar en cualquier rango, se elige 40.1 para asociarlo de manera más facil con la VLAN 40.

```

## Configuración del DHCP server

Esto debe ser aplicado en el switch que deseas que actue como DHCP server, en nuesto caso, SW1-C 

``` bash
SW1-C(config)$ ip dhcp pool VLAN40
SW1-C(config)$ network 192.168.40.0 255.255.255.0
SW1-C(config)$ default-router 192.168.40.1
SW1-C(config)$ dns-server 8.8.8.8
SW1-C(config)$ exit
```

## Docker y DHCP 
Si bien ahora deberiamos tener todo correctamente configurado para que nuestros hosts reciban una dirección IP asignada de forma dinamica. Existen ciertos problemas relacionados con docker que se debe resolver de manera manual antes de poder probar DHCP correctamente. 
Esto es abordado en ...

#TODO incluir una entrada con el problema de DHCP y docker