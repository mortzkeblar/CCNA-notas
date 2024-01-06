![](_anexos_/Screenshot%20from%202023-12-27%2012-24-31.png)

*GNS3*:

![](_anexos_/Screenshot%20from%202023-12-27%2012-25-28.png)

Configuramos el switch para agregar los hosts a sus respectivas VLANs.

``` bash
SW1$ conf t
SW1(config)$ vlan 10 # hacemos lo mismo con vlan 20 y 30

# Asignamos las interfaces
SW1(config)$ int e3/1
SW1(config-if)$ sw mod acc
SW1(config-if)$ sw ac vlan 10
## Los mismo pasos para vlan20 -> e3/2 y vlan30 -> e3/3
```

Entre R1 y SW1 solo tenemos un cable que une `e0/0` en ambos lados. Para poder tener enrutamiento debemos generar interfaces virtuales en el router y asignar esas interfaces a las VLAN correspondientes.

``` bash
# Router (R1)
R1(config)$ int e0/0
R1(config-if)$ no shutdown

# Por lo general (y conveniencia) el numero de la subinterfaz virtual es el mismo que le de la VLAN
R1(config)$ int e0/0.10
# Configuramos a que VLAN pertenece este subinterfaz, el match con el numero de VLAN es obligatorio
R1(config-subif)$ encapsulation dot1q 10
R1(config-subif)$ ip add 192.168.10.1 255.255.255.0

# Seguimos los mismo pasos para e0/0.20 y e0/0.30
```

Por ultimo configuramos la puerta del switch que llega al router. 

``` bash
SW1(config)$ int e0/0
SW1(config-if)$ no shut
SW1(config-if)$ switchport mode trunk
Command rejected: An interface whose trunk encapsulation is "Auto" can not be configured to "trunk" mode.

SW1(config-if)$ switchport trunk encapsulation dot1q
SW1(config-if)$ switchport mode trunk
SW1(config-if)$ do show interface trunk
...

```

Con esto podemos llegar desde los hosts hasta el router, también entre los hosts. 

> Este metodo no es recomendable, porque si bien es escalable a nivel de hosts. Se genera congestión en el enlace troncal (bottle neck) lo cual puede generar un mal performance.


Ver: [VLAN mismatch](VLAN/VLAN%20mismatch.md)