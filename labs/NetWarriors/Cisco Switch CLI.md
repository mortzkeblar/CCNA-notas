---
tags:
  - general
  - CCNA
---

Secuencia de arranques de un switch
- POST (autodiagnostico)
- Cargador de arranque 
- IOS


``` bash
# Ver los directorios almacenados en la flash dir
Switch$ dir flash:
```

VLAN administrable
- Se debe configurar una VLAN diferente a la VLAN 1
- Para poder administrar debemos darle al switch una dirección IP
	- Esta IP es para administración, no enrutamiento
- Se da enfasis en los switches y no en los routers porque en ese caso, es mucho mas facil acceder a traves de las IPs configurada en alguna de sus interfaces
	- En cambio, en un switch debemos configurar necesariamente un dirección IP que debe estar en el rango de la red de la LAN

``` bash
# Configuración de una vlan para administrar desde un dispositivo proveniente de una interfaz especifica
Switch(config)$ vlan 99
Switch(config-vlan)$ name ADMIN
Switch(config-vlan)$ exit

## En este caso agregamos el dispositivo desde el que queremos administrar el equipo a la vlan 99
Switch(config)$ int f0/1
Switch(config-if)$ switchport mode access 
Switch(config-if)$ switchport access vlan 99
Switch(config-if)$ exit
```

``` bash
# Direccionamiento IP
Switch(config)$ interface vlan 99
Switch(config-if)$ ip address 192.168.1.2 255.255.255.0
Switch(config-if)$ no shutdown

# Definimos un default-gateway para administracion remota
Switch(config)$ ip default-gateway 192.168.1.2
```

En switchport de L2 tenemos dos modos:
- access, conectados a puntos finales
- trunk, conectados a otros dispositivos de infraestructura de la red

``` bash
# Configuracion para levantar el protocolo telnet
Switch(config)$ line vty 0 15
Switch(config-if)$ password cisco
Switch(config-if)$ login
Switch(config-if)$ logging synchronous
Switch(config-if)$ exec-timeout 20
Switch(config-if)$ exit
```

``` bash
# Guardar la conf
Switch(config-if)$ copy running-config starup-config
```

