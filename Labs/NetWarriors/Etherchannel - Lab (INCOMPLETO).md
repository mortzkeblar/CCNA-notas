---
tags:
  - ETHERCHANNEL
  - lab
  - CCNA
date created: Saturday, October 19th 2024, 11:08:14 pm
date modified: Wednesday, November 20th 2024, 11:33:50 pm
---

#TODO completar

``` bash 
# Comandos utiles para depuración 

NC-LAN1(config-if)#do sh int port-channel 3
NC-LAN1(config-if)#do sh etherchannel summary
```


Vamos a configurar [(LEGACY)Etherchannel]((LEGACY)Etherchannel.md) primeramente en NC-LAN1 que pertenece al Core Central y asociamos las tres interfaces conectadas hacia SDC-1.

``` bash
NC-LAN1> en
NC-LAN1#conf t
NC-LAN1(config)#int range g0/0-2
NC-LAN1(config-if-range)#channel-group 3 mode active # Activamos LACP en este caso
```

Ahora podemos ingresar directamente a configurar la interface como si fuera una interface fisica. Esto nos ahorra mucho tiempo ya que solo debemos configurar una interface para que eso se replique en todos sus puertos. 

``` bash
NC-LAN1(config)#int port-channel 3
```

Aprovechando que estamos en el port 3, vamos a configurar el trunk port y habilitamos el trafico a todas las VLANs con las que vamos a trabajar. 

``` bash
NC-LAN1(config-if)#switch trun encap dot1q
NC-LAN1(config-if)#switc mode trunk
NC-LAN1(config-if)#switc trunk allowed vlan 77,100,101,102,111,120,121
```

Una vez hecho esto, podemos ver que tenemos configurado todo correctamente con estos dos comandos: `show interface port-channel 3` y `show etherchannel summary` que brinda información más corta y probablemente más util en este momento.

``` bash
NC-LAN1(config-if)#do sh int port-channel 3
Port-channel3 is down, line protocol is down (notconnect) 
  Hardware is EtherChannel, address is 0000.0000.0000 (bia 0000.0000.0000)
  MTU 1500 bytes, BW 10000 Kbit/sec, DLY 1000 usec, 
     reliability 255/255, txload 1/255, rxload 1/255
  Encapsulation ARPA, loopback not set
  Keepalive set (10 sec)
  ARP type: ARPA, ARP Timeout 04:00:00
  Last input never, output never, output hang never
  Last clearing of "show interface" counters never
  Input queue: 0/2000/0/0 (size/max/drops/flushes); Total output drops: 0
  Queueing strategy: fifo
  Output queue: 0/40 (size/max)
  5 minute input rate 0 bits/sec, 0 packets/sec
  5 minute output rate 0 bits/sec, 0 packets/sec
     0 packets input, 0 bytes, 0 no buffer
     Received 0 broadcasts (0 multicasts)
     0 runts, 0 giants, 0 throttles 
     0 input errors, 0 CRC, 0 frame, 0 overrun, 0 ignored
     0 input packets with dribble condition detected
     0 packets output, 0 bytes, 0 underruns
     0 output errors, 0 collisions, 0 interface resets
     0 unknown protocol drops 
...

NC-LAN1(config-if)#do sh etherchannel summary
Flags:  D - down        P - bundled in port-channel
        I - stand-alone s - suspended
        H - Hot-standby (LACP only)
        R - Layer3      S - Layer2
        U - in use      N - not in use, no aggregation
        f - failed to allocate aggregator

        M - not in use, minimum links not met
        m - not in use, port not aggregated due to minimum links not met
        u - unsuitable for bundling
        w - waiting to be aggregated
        d - default port

        A - formed by Auto LAG


Number of channel-groups in use: 1
Number of aggregators:           1

Group  Port-channel  Protocol    Ports
------+-------------+-----------+-----------------------------------------------
3      Po3(SD)         LACP      Gi0/0(s)    Gi0/1(s)    Gi0/2(s)    

NC-LAN1(config-if)#~
```

Como pueden ver, estamos en estado `SD` (Layer2 y Down), eso es porque [LACP (Link Aggregation Control Protocol)](LACP%20(Link%20Aggregation%20Control%20Protocol).md)  se encarga de verificar que ambos extremos esten configurados correctamente para poder habilitar etherchannel. Dicho esto nos vamos al otro extremo de la interface que configuramos, es decir SDC-1 para realizar el mismo procedimiento.  

``` bash
SDC-1#conf t
SDC-1(config)#int range g0/0-2
SDC-1(config-if-range)#channel-group 3 mode active
SDC-1(config-if-range)#exit

SDC-1(config)#interface port-channel 3
SDC-1(config-if)#switchp trunk encap dot1q
SDC-1(config-if)#switch mode trunk
SDC-1(config-if)#switc trunk allowed vlan 77,100,101,102,111,120,121

```

Voy a realizar esta misma configuración para enlazar a tráves de trunk, NC-LAN2 con SDC-1, esta vez para crear el grupo 4. 

``` bash
# NC-LAN2

NC-LAN2#conf t
NC-LAN2(config)#int range g0/0-2
NC-LAN2(config-if-range)#channel-group 4 mode active 
NC-LAN2(config-if-range)#exit
NC-LAN2(config)#
NC-LAN2(config)#int port-channel 4
NC-LAN2(config-if)#switchp trun encapsulation dot1q 
NC-LAN2(config-if)#switchp mod trunk 
NC-LAN2(config-if)#switchp trun allow vlan 77,100,101,102,111,120,12

# Hacemos lo mismo con SDC-1 en los puertos que corresponden

SDC-1#conf t
SDC-1(config)#int range g1/0-2
SDC-1(config-if-range)#channel-group 4 mode active 
SDC-1(config-if-range)#exit
SDC-1(config)#int port-channel 4
SDC-1(config-if)#switchp trun encapsulation dot1q 
SDC-1(config-if)#switchp mod trunk 
SDC-1(config-if)#switchp trun allow vlan 77,100,101,102,111,120,121

# Verificamos en SDC-1

SDC-1(config-if)#do sh ethercha summ
...

Number of channel-groups in use: 2
Number of aggregators:           2

Group  Port-channel  Protocol    Ports
------+-------------+-----------+-----------------------------------------------
3      Po3(SU)         LACP      Gi0/0(P)    Gi0/1(P)    Gi0/2(P)    
4      Po4(SU)         LACP      Gi1/0(P)    Gi1/1(P)    Gi1/2(P)    


## Podemos ver a los channel-group 3 y 4 creados y activos


```

Antes de pasar a otros switches para configuración, creamos las VLAN en los switches que forman parte del core para la distribución por los demas dispositivos.

``` bash
# Conf en NC-LAN1, hacer lo mismo para NC-LAN2 
NC-LAN1(config)#vlan 40
NC-LAN1(config-vlan)#vlan 77
NC-LAN1(config-vlan)#vlan 100 
NC-LAN1(config-vlan)#vlan 101 
NC-LAN1(config-vlan)#vlan 102 
NC-LAN1(config-vlan)#vlan 111 
NC-LAN1(config-vlan)#vlan 120 
NC-LAN1(config-vlan)#vlan 121

```

Ahora pasamos a configurar los tres switches que estan cerca a la capa de acceso, me refiero a E1SW1, E1SW2 y E1SW3. Cada número de channel se encuentra  en la imagen.

``` bash
en
conf t
int range 
```





```

conf t
int range g1/0-2
channel-group 4 mode active 
exit

int port-channel 4
switchp trun encapsulation dot1q 
switchp mod trunk 
switchp trun allow vlan 77,100,101,102,111,120,121

vlan 40
vlan 77
vlan 100 
vlan 101 
vlan 102 
vlan 111 
vlan 120 
vlan 121
```


| UBICACIÓN | VLAN | NOMBRE | DIRECCIONAMIENTO | SUBRED | COMENTARIOS | DOMINIO |
| ---- | ---- | ---- | ---- | ---- | ---- | ---- |
| Data Center | 40 | Servidores | 10.0.40.0 - 10.0.40.31 | 255.255.255.224 | No anunciada por VTP | pyme.com |
| Edificio 1 | 100 | RRHH | 10.0.10.0 - 10.0.10.127 |  |  |  |
| Edificio 1 | 101 | Gerencia | 10.0.10.128 - 10.0.10.255 | 255.255.255.128 |  |  |
| Edificio 1 | 102 | Proyectos | 10.0.12.0 - 10.0.12.127 | 255.255.255.128 |  |  |
| Edificio 1 | 120 | Datos Inalambrica | 10.0.20.0 - 10.0.20.127 | 255.255.255.128 |  |  |
| Edificio 1 | 121 | Voz Inalambrica  | 10.0.20.128 - 10.0.20.191 | 255.255.255.192 | 63 libres |  |
| Edificio 1 | 77 | ADMIN | 10.0.7.0 - 10.0.7.255 | 255.255.255.0 |  |  |
| Edificio 1 | 111 | NATIVA |  |  |  |  |
