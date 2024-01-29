Vamos a configurar RIPv1  (classful) y [RIPv2](RIPv2.md) (classless). Abajo esta la configuración para R1, sera la misma para R2 pero necesitará la dirección IP `.2` y no necesitará anunciar la red `172`.

``` bash
_R1#conf t_
_R1(config)#int f0/0_
_R1(config-if)#ip add 192.168.1.1 255.255.255.240_
_R1(config-if)#no shut_
_R1(config-if)#int lo0_
_R1(config-if)#ip add 172.16.1.1 255.255.255.0_
_R1(config-if)#router rip_
_R1(config-router)#network 172.16.1.0_
_R1(config-router)#network 192.168.1.0_
```

Si configuraste R2 correctamente, deberias poder ver la red en el `Loopback` de R1 anunciada. 

``` bash
_R2#show ip route_
_Codes: C – connected, S – static, R – RIP, M – mobile, B – BGP_
_[output truncated]_
_Gateway of last resort is not set_
_**R    172.16.0.0/16 [120/1] via 192.168.1.1, 00:00:01, FastEthernet0/0**_
_192.168.1.0/28 is subnetted, 1 subnets_
_C       192.168.1.0 is directly connected, FastEthernet0/0_
```

Debido a que RIP es classful, no sera conciente de VLSM. Entonces, no sera necesario informar sobre la subnet mask.  Podemos ver con `debug` en R2 como las redes se anuncian mediante broadcast y que no hay información sobre la subnet mask. 

``` bash
_*Mar  1 00:05:27.819: RIP: sending v1 flash update to_
_**255.255.255.255** via FastEthernet0/0 (192.168.1.1)_
_*Mar  1 00:05:27.819: RIP: build flash update entries_
_*Mar  1 00:05:27.819:      **network 172.16.0.0 metric 1**_
```

En cambio, un protocolo classless no hace presunciones y siempre anunciara la subnet mask con la actualización de routing. Por esta razon, es libre de elegir la subnet mask que desee independientemente de la subnet mask por defecto definidas en las redes de clase A, B y C. 

``` bash
# Habilitamos el protocolo classless RIPv2
_R2(config)#router rip_
_R2(config-router)#version 2_
_R2(config-router)#end_
```

Si ejecutamos el debug nuevamente, vemos que las actualizaciones son multicast y que nos muestra la información de subnet mask.

``` bash
_R2#_
_*Mar  1 06:15:40.406: RIP: received v2 update from 192.168.1.1 on FastEthernet0/0_
_*Mar  1 06:15:40.406:      172.16.0.0/16 via 0.0.0.0 in 1 hops_
_R2#_
_*Mar  1 06:15:42.906: RIP: sending v2 update to 224.0.0.9 via FastEthernet0/0 (192.168.1.2)_
```

La red `172` todavia se muestra en `/16` (Class B) en nuestra tabla. cuando nosotros configuramos la IP al principio en `/24`. Esto se llama _summarization_, algunos protocolos hacen esto por defecto. Podemos desactivarlo con:

``` bash
_R1(config)#router rip_
_R1(config-router)#no auto-summary_
_R1(config-router)#end_
_Now R2 receives the /24 mask:_
_R2#_
_*Mar  1 06:18:33.586: RIP: received v2 update from 192.168.1.1 on FastEthernet0/0_
_*Mar  1 06:18:33.586:      **172.16.1.0/24** via 0.0.0.0 in 1 hops_
```
