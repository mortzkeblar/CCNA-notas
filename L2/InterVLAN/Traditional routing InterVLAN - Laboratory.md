---
tags:
  - VLAN
  - CCNA
---
#TODO agregar teoria
## Enrutamiento tradicional

![](_anexos_/Screenshot%20from%202023-12-27%2010-43-58.png)

## Configuración de los hosts
Asignamos las IPs a los tres hosts.

``` bash
VLAN10(config)$ int e0/0
VLAN10(config-if)$ no shut
VLAN10(config-if)$ ip add 192.168.10.100 255.255.255.0

VLAN20(config)$ int e0/0
VLAN20(config-if)$ no shut
VLAN20(config-if)$ ip add 192.168.20.100 255.255.255.0

VLAN30(config)$ int e0/0
VLAN30(config-if)$ no shut
VLAN30(config-if)$ ip add 192.168.30.100 255.255.255.0
```

- Aun no se puede hacer ping entre los hosts porque aun no estan conectadas en el enrutamiento

## Configuración de los switches

> A veces pasa que no se crean las VLAN correctamente , en este caso la creamos nuevamente y deberia aparecer en el `show brief`. Esto se debe a un problema en el emulador de GNS3.

``` bash
IOU2$ conf t
IOU2(config)$ vlan 10
IOU2(config-vlan)$ name Usuarios
IOU2(config-vlan)$ vlan 20
IOU2(config-vlan)$ name Admin
IOU2(config-vlan)$ vlan 30
IOU2(config-vlan)$ name Externos
IOU2(config-vlan)$ do show vlan brief  # verificamos que los vlan estan creados

# En el switch 2 no es necesario crear la vlan 30 (porque no hay ningun dispositivo que este usando esa VLAN) pero por contundencia y redundancia de la red creamos las 3 VLANs

IOU3$ conf t
IOU3(config)$ vlan 10
IOU3(config-vlan)$ vlan 20
IOU3(config-vlan)$ vlan 30
IOU3(config-vlan)$ do sh vlan brief
```

Ahora asignamos las interfaces de los switches (que estan conectados a los hosts) a los VLANs correspondientes.

``` bash

# IOU2

IOU2(config)$ int e0/0
IOU2(config-if)$ no shutdown # opcional en switches
IOU2(config-if)$ description SERVIDOR VLAN10
IOU2(config-if)$ switchport mode access
IOU2(config-if)$ switchport access vlan 10

# Aplicamos lo mismo para vlan 20
```

Para poder comunicar los hosts que estan en `VLAN10` y `VLAN20` debemos aun, configurar el router. Cuestiones a considerar.
- `e0/2` de IOU2 esta conectado a `e0/1` de R1
- `e0/3` de IOU2 esta conectado `e0/2` de R1
Cada una de esas interfaces deben ser asignados a una VLAN correspondiente.
- `e0/2` -> VLAN10
- `e0/3` -> VLAN20

``` bash
IOU2(config-if)$ int e0/2
IOU2(config-if)$ switchport mode access
IOU2(config-if)$ switchport access vlan 10

IOU2(config-if)$ int e0/3
IOU2(config-if)$ switchport mode access
IOU2(config-if)$ switchport access vlan 20

IOU2(config-if)$ do show vlan brief
...
10 Usuarios     active     Et0/0, Et0/2
20 Admin        active     Et0/1, Et0/3
...
```

## Configuración del router
``` bash
# R1

# Asignamos una IP que sirva como puerta de enlace para todos los hosts de la VLAN 10, en este caso la primera IP que seria 192.168.10.1 
R1$ conf t
R1(config)$ int e0/1
R1(config)$ no shutdown # obligatorio en routers
R1(config)$ description VLAN10
R1(config)$ ip address 192.168.10.1 255.255.255.0

# Realizamos los mismos pasos pero con e0/2 asignado a la ip 192.168.20.1

# NOTA: cuando usamos este tipo de configuración, no es necesario asignar una VLAN al router
```

En este punto debería haber conectividad de VLAN10 y VLAN20 hacia el router. Lo cual se 
puede verificar con `ping`.

``` bash
# VLAN10 host
VLAN10$ ping 192.168.10.1

# VLAN20 host
VLAN20$ ping 192.168.20.1
```

Pero aun no podemos conectar los hosts de las VLANs. Eso ocurre porque no hay una ruta configurada para poder llegar al otro hosts esto puede verificarse con `show ip route`.

``` bash
VLAN10(config)$ ip route 0.0.0.0 0.0.0.0 192.168.10.1

# aplicar los mismo con VLAN20 y la ip asignada del router
```

Con esto ya es posible conectar las dos VLAN. La cual sigue esta ruta.
``` bash
VLAN10 (e0/0) -> (e0/0) Switch IOU2 (e0/2) -> (e0/1) R1 (e0/2) -> (e0/3) Switch IOU2 (e0/1) -> VLAN20 (e0/0)

# Es decir, esta pasando dos veces por el mismo switch ya que como son VLANs separadas solo pueden comunicarse a través del router.
```

> Esta no es una buena solución. Sobretodo cuando las VLANs comienzan a crecer, si tenemos 50 VLANs por ejemplo, implica tener 50 interfaces conectadas al router.
> Considerar que los routers solo suelen tener una cantidad baja de interfaces (entre 2 y 5 aprox). 
