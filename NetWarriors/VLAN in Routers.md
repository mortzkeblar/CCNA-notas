---
tags:
  - VLAN
  - configuration
  - lab
---



``` bash
# Comandos utiles para depuración
Router$ show ip interface brief

Switch$ show vlan brief 

Switch$ show interfaces trunk

```


El proposito de este laboratorio es el de poder crear y enrutar VLANs para poder conectarlas a través de routing.

Debemos crear tres VLANs con diferentes rangos IP. Estas se comunican a través del router.
- 200
- 201
- 202

![](_anexos_/Screenshot%20from%202024-01-11%2003-16-12.png)


## Creación de VLANs, access mode
Primero configuramos las VLAN con el `mode access` en las distintas interfaces que llegan a los hosts finales

``` bash
# p. ej. configuración para la interface 0/2 en SW1 con la VLAN 202
SW1$ conf t
SW1(config)$ int g0/2
SW1(config)$ switchport mode access 
SW1(config)$ switchport access vlan 202
SW1(config)$ exit

## Realizar los mismos pasos en las interfaces de los otros SWs que correspondan 
```

## Switches y Router, trunk mode
Ahora que tenemos configuradas las VLAN en las interfaces que va hacia el host, es momento de configurar los enlaces troncales en los switches para poder conectarlas con el router

``` bash
# p. ej. configuración de la interface 0/0 de SW1 que conecta con 0/1 en R1
SW1$ int g0/0
SW1(config)$ no shut
SW1(config)$ switchport trunk encapsulation dot1q
SW1(config)$ switchport mode trunk 
SW1(config)$ switchport trunk allowed vlan all # Para permitir el acceso o negar el trafico de VLANs 
SW1(config)$ do show interces trunk

```

### Router, trunk mode
Por ultimo configuramos el router con creación de sub interfaces (o interfaces lógicas) sobre las interfaces físicas para cada VLAN y su correspondiente enrutamiento

``` bash
# p. ej. configuración de la interface 0/1 de R1 que conecta con SW1
router$ int g0/1
router(config)$ no shut
router(config)$ int g0/1.201
router(config)$ encapsulation dot1q 201
router(config)$ ip address 192.168.201.1 255.255.255.0
router(config)$ do show ip interface brief
```

- No olvidar sal configurar un router el comando `no shutdown`, considerar que las interfaces de un router por defecto siempre estan en `off`
- La sub interface que creamos no es necesario que sea el mismo numero que la VLAN, pero si es recomendable para que sea ordenado y facil de identificar
- En el comando `encapsulation dot1q 201`, es obligatorio que el numero coincida con la VLAN que estamos configurando en esa subinterface 

## Configuración final, hosts 
Hasta este punto tenemos configurado la red para poder utilizar VLANs distintas conectadas entre si. Este apartado abarca la configuración de IP para los hosts finales. 
``` bash
# p. ej. configuración del host nettools-1 qque esta conectado a SW2 
## Configuramos la IP segun configuremos la sub interface del router, en este caso usamos la red 192.168.200.0/24
$ ip addr add 192.168.200.101 # .101 es un numero arbitrario, se puede configurar cualquiera en realidad, excepto .1 que es el gateway del router por su puesto
$ ip route add default via 192.168.200.1 dev eth0

# El ultimo comando es muy importante ya que si no configuramos el gateway el host no va a poder encontrar las redes de las otras VLAN.
```

Una vez configurado los hosts deberíamos ser capaces de alcanzar, no solo las IPs de las distintas VLAN configuradas en el router (y que actuan como gateway) sino llegar a  los hosts de cada VLAN. 



