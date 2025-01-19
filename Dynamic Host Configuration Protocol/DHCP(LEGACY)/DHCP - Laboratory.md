---
tags:
  - DHCP
  - lab
  - CCNA
date created: Saturday, October 19th 2024, 11:08:14 pm
date modified: Wednesday, November 20th 2024, 11:33:50 pm
---


![](Screenshot%20from%202024-01-01%2023-03-50.png)

##### Objetivos
- Configurar un router Cisco como servidor DHCP de una o más redes locales
- Configurar la opción de DHCP relay o `ip helper-address` con un servidor DHCP remoto
##### Instrucciones
1. R1 debe proveer de DHCP a la VLAN10 y VLAN20. La VLAN10 debe tener asignado un tiempo de concesión de 10 horas. No se deben incluir las primeras 10 IP de cada bloque IP en ambas VLANs.
2. PC3 debe recibir el direccionamiento IP correcto que le permita conectarse al resto de la red. Para la red 192.168.30.0/24 el servidor DHCP debe ser la máquina remota conectada a R4. 
##### Solución
``` bash
# Solución 1 - VLAN10
R1(config)$ ip dhcp pool VLAN10
R1(dhcp-config)$ network 192.168.10.0/24
R1(dhcp-config)$ default-router 192.168.10.1

R1(dhcp-config)$ dns-server 8.8.8.8
R1(dhcp-config)$ exit

R1(config)$ ip dhcp excluded-address 192.168.10.1 192.168.10.10

## Configurar el tiempo de conseción
R1(config)$ ip dhcp pool VLAN10
R1(dhcp-config)$ lease 0 10
## Para ver el resumen de configuración
R1(config)$ show running-config | section ip dhcp

### Para comprobar la solución hacer ping desde PC1 hacia DHCP server
## Ejecutar los mismos pasos para VLAN20

```

- Cuando PC3 quiere enviar una consulta DHCP discover, lo cual le permita llegar a todos los puntos conectados en la LAN incluyendo f1/0 en el router R2.
	- La idea es configurar el router R2 para que al recibir la solicitudes _DHCP discover_. Pueda reenviarlo en formato _unicast_ hacia el servidor DHCP.

``` bash
# Solución 2
R1(config)$ int f1/0
## Es importante asegurarse que el DHCP server sea alcanzable desde el router
R1(config-if)$ ip helper-address 10.120.0.230 
```
