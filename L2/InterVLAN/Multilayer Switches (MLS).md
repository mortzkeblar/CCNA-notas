---
tags:
  - VLAN
  
  
---
#TODO agregar teoria

_SVI: Switch VLAN interface (VLAN administrativa)_

![](_anexos_/Screenshot%20from%202023-12-27%2013-15-58.png)

- La diferencia entre un switch comun y uno multilayer es que los mls si puede revisar, procesar y trabajar con los paquetes del protocolo IP al igual que un router y tomar decisiones de enrutamiento. 
- Los switches MLS tienen unas tablas denominadas _[TCAM](https://www.ciscopress.com/articles/article.asp?p=101629&seqNum=4)_ y un sistema de conmutación llamado _[CEF](https://www.cisco.com/c/en/us/support/docs/routers/12000-series-routers/47321-ciscoef.html)_ que permite trabajar a un nivel, denominado _wirespeed_. Eso significa que la conmutación a nivel de capa 3 desde una VLAN a otra. Se hace una velocidad de la electronica que tenga el dispositivo. 
	- NOTA: _wirespeed_ no hace referencia a la velecidad de transferencia del cable. 

_GNS3:_

![](_anexos_/Screenshot%20from%202023-12-27%2013-26-14.png)

``` bash
# configuración del switch MLS
IOU1$ conf t
# creamos interfaces virtuales (svi), una por vlan
IOU1(config)$ interface vlan 10
IOU1(config-if)$ no shutdown
```

Si revisamos las interfaces: 
```
IOU1(config-if)$ do show ip int brief
...
Vlan10   unassigned  YES unset   down  down
...

## Aun se muestra en estado 'down' a pesar de ejecutar 'no shutdown'.
``` 

Esto ocurre porque creamos las interfaces pero no las VLANs aun. 
``` bash
IOU1(config-if)$ vlan 10
# Los mismo con vlan20 y 30

IOU1(config-if)$ do show vlan brief

# Despues de crear las VLAN las interfaces deberia pasar a 'up' automaticamente. 
```

Ahora toca asignar cada interfaz creada en `mode access`.
``` bash
IOU1(config)$ int e0/1
IOU1(config-if)$ switch mode access
IOU1(config-if)$ switch access vlan 10

# Aplicamos lo mismo para vlan20 -> e0/2 y vlan30 -> e0/3
```

Por ultimo asignamos las direcciones IP a cada VLAN
``` bash
IOU1(config-if)$ int vlan 10
IOU1(config-if)$ ip add 192.168.10.1 255.255.255.0

# Aplicamos lo mismo para vlan20 y vlan30

IOU1(config-if)$ do show ip int brief
...
```

Con esto podemos conectarnos entre los hosts de los distintos VLANs.

> NOTA: para verificar que el enrutamiento funcione correctamente:
> - Cada dirección IP que uno configura en el SVI correspondiente `int vlan 10` en el switch. Debe ser el `gateway` de los hosts. 
> - Por default, un switch MLS se comporta igual que un switch de capa 2. Para habilitar las funciones relacionadas con la capa tres, ejecutar `ip routing`. 