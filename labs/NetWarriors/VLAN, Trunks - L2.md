---
tags:
  - VLAN
  - lab
  - CCNA
date created: Saturday, October 19th 2024, 11:08:14 pm
date modified: Wednesday, November 20th 2024, 11:33:50 pm
---

https://www.youtube.com/watch?v=dpTCpqZH3zY&list=PLoqM5eIpDpUEhtUNKuOd-sGll7NrvlwD7&index=15

![](_anexos_/Screenshot%20from%202024-01-09%2016-49-08.png)

Notas del  video:
- El objetivo de las VLAN es el de segmentar el trafico de la red a nivel de L2.
- La VLAN por defecto (1) y la nativa es la misma.
- Cuando configuramos `n` VLANs, estamos creando `n` redes diferentes. Esto es consecuencia del primer punto
	- Esto implica que en los dispositivos L3, sea necesario el enrutamiento entre VLANs

# Configuración de VLANs
Creamos las VLANs en el primer switch
``` bash
Switch(config)$ vlan 10
Switch(config-vlan)$name DEPARTAMENTO_A
Switch(config-vlan)$exit
Switch(config)$ vlan 20
Switch(config-vlan)$ name DEPARTAMENTO_B
Switch(config-vlan)$ exit
Switch(config)$ vlan 30
Switch(config-vlan)$ name DEPARTAMENTO_C
Switch(config-vlan)$ exit

# Verificamos que las VLAN esten creadas
Switch(config)$ do show vlan brief
VLAN Name                             Status    Ports
---- -------------------------------- --------- -------------------------------
1    default                          active    Gi0/0, Gi0/1, Gi0/2, Gi0/3
                                                Gi1/0, Gi1/1, Gi1/2, Gi1/3
                                                Gi2/0, Gi2/1, Gi2/2, Gi2/3
                                                Gi3/0, Gi3/1, Gi3/2, Gi3/3
10   DEPARTAMENTO_A                   active    
20   DEPARTAMENTO_B                   active    
30   DEPARTAMENTO_C                   active    
1002 fddi-default                     act/unsup 
1003 token-ring-default               act/unsup 
1004 fddinet-default                  act/unsup 
1005 trnet-default                    act/unsup 

# Tenemos que realizar los mismos pasos para el otro switch

```

- Para no tener la necesidad de configurar las VLAN por cada switch, se puede hacer uso del protocolo VTP

Si bien en este ejercicio vamos a configurar una VLAN por cada puerto, eso no quiere decir que cada puerto debe tener una VLAN diferente.
- Todas las interfaces de un switch puede pertenecer a una sola VLAN, por lo general esta forma es bastante comun en un entorno real.

``` bash
# ahora craamos la VLAN en cada interface con accedss mode
Switch$ conf t
Switch(config)$ int g0/1
Switch(config-if)$ switchport mode access
Switch(config-if)$ switchport access vlan 10

## hacer lo mismo con VLAN 20 y 30, en los dos switches que estan conectados a los end hosts
```

Si tratamos de hacer ping entre hosts de los diferentes switches vemos que no responden a las solicitudes ICMP, esto se debe:
- Las VLAN solo realizan el trafico de la red por los enlaces troncales (trunks)

Como tenemos al switch MLS conectado a los dos switches de abajo. Configuramos ese switch para generar los trunks links. 

``` bash
# Seleccionamos las interfaces 0 y 1
Switch$ conf t
Switch(config)$ int g0/0-1
Switch(config-if-range)$ switchport trunk encapsulation dot1q
Switch(config-if-range)$ switchport mode trunk
Switch(config-if-range)$ switchport trunk allowed vlan 10,20,30  # tambien es posible usar el parametro 'all'
```

Ahora solo queda configurar los hosts con IPs estaticas que esten en el mismo rango y en la misma VLAN.
En este caso tenemos los dos contenedores `nettools-1` y `nettools-4` estan configurados en la misma VLAN (10), entonces configuramos las IPs.

``` bash
$ ip addr add 192.168.0.10/24 dev eth0

# Hacer lo mismo en la maquina 4 pero con IP que termina en .14 (en realidad cualquier numero pero para respetar el orden... )
```

Ahora deberiamos poder hacer ping desde 192.168.0.10 hacia 192.168.0.14 y viceversa.

``` bash
root@debian-gns3-endhost-nettools-4 /# ping 192.168.0.10
PING 192.168.0.10 (192.168.0.10) 56(84) bytes of data.
64 bytes from 192.168.0.10: icmp_seq=1 ttl=64 time=11.0 ms
64 bytes from 192.168.0.10: icmp_seq=2 ttl=64 time=3.89 ms
64 bytes from 192.168.0.10: icmp_seq=3 ttl=64 time=4.22 ms
64 bytes from 192.168.0.10: icmp_seq=4 ttl=64 time=6.11 ms
64 bytes from 192.168.0.10: icmp_seq=5 ttl=64 time=5.26 ms
c64 bytes from 192.168.0.10: icmp_seq=6 ttl=64 time=3.60 ms
^C
--- 192.168.0.10 ping statistics ---
6 packets transmitted, 6 received, 0% packet loss, time 5006ms
rtt min/avg/max/mdev = 3.599/5.681/11.015/2.533 ms
root@debian-gns3-endhost-nettools-4 /# 

```

