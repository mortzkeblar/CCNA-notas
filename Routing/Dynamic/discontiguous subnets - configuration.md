
En la imagen se muestra una topologia mal diseñada. Especificamente la red `10`, esa forma de dividir la red se conoce como discontiguous subnets. 
Esto es un problema porque R2 intentará enviar cualquier tráfico por la red 10 por cualquiera de las interfaces, lo que provocara una perdida de paquetes del 50% o más. 

![](anexos/14-5.png)

Podemos ver que tenemos [auto summarization](auto%20summarization.md) en R1.
``` bash
R1#show ip protocols
Routing Protocol is “rip”
Outgoing update filter list for all interfaces is not set
Incoming update filter list for all interfaces is not set
Sending updates every 30 seconds, next due in 18 seconds
Invalid after 180 seconds, holddown 180, flushed after 240
Redistributing: rip
Default version control: send version 2, receive version 2
Interface             Send  Recv  Triggered RIP  Key-chain
Serial0/0             2     2
Loopback0             2     2
Loopback1             2     2
Loopback2             2     2
Automatic network summarization is in effect
Maximum path: 4
Routing for Networks:
10.0.0.0
    172.16.0.0
Routing Information Sources:
Gateway         Distance      Last Update
172.16.1.2      120           00:00:24
Distance: (default is 120)
```

R2 enviará el trafico destinado hacia las redes `10` fuera de las interfaces serial, porque no sabe o no entiende que la red `10` esta discontigua. Según muestra esta salida.

> nota personal: entiendo que el problema aqui se relaciona con que 10.0.0.0/8 tiene dos next-hop distintos. 

``` bash
R2#show ip route
Gateway of last resort is not set
C    172.16.0.0/16 is directly connected, Serial0/0
R    10.0.0.0/8 [120/1] via 192.168.1.2, 00:00:23, Serial0/1
                [120/1] via 172.16.1.1, 00:00:10, Serial0/0
C    192.168.1.0/24 is directly connected, Serial0/1
[output truncated]
```

No podemos hacer ping hacia ninguna red `10` porque falla. El comando `show ip route 10.10.30.1` nos muestra el origen del problema.
``` bash
R2#ping 10.10.30.1
Type escape sequence to abort.
Sending 5, 100-byte ICMP Echos to 10.10.30.1, timeout is 2 seconds:
U.U.U
Success rate is 0 percent (0/5)

R2#show ip route 10.10.30.1
Routing entry for 10.0.0.0/8
Known via “rip”, distance 120, metric 1
Redistributing via rip
Last update from 172.16.1.1 on Serial0/0, 00:00:01 ago
Routing Descriptor Blocks:
192.168.1.2, from 192.168.1.2, 00:00:11 ago, via Serial0/1
Route metric is 1, traffic share count is 1
* 172.16.1.1, from 172.16.1.1, 00:00:01 ago, via Serial0/0
Route metric is 1, traffic share count is 1
```

El `*` junto a `172.16.1.1` nos indica que el siguiente paquete usará el next-hop en 172.16.1.1 porque RIP esta haciendo load balancing sobre lo que piensa son dos enlaces de red. Para solucionar esto debemos desactivar el [auto summarization](auto%20summarization.md).

``` bash
R2(config)#router rip
R2(config-router)#no auto-summary
```

#exam Esto debe ser aplicado en todos los routers afectados, cualquier router sin aplicar mantendrá la [auto summarization](auto%20summarization.md) (cuidado con esto en el examen). 

``` bash
R2#show ip route
Gateway of last resort is not set
C    172.16.0.0/16 is directly connected, Serial0/0
10.0.0.0/8 is variably subnetted, 6 subnets, 2 masks
R       10.10.10.0/24 [120/1] via 172.16.1.1, 00:00:06, Serial0/0
R       10.0.0.0/8 [120/1] via 192.168.1.2, 00:02:31, Serial0/1
[120/1] via 172.16.1.1, 00:01:32, Serial0/0
R       10.10.20.0/24 [120/1] via 172.16.1.1, 00:00:06, Serial0/0
R       10.10.30.0/24 [120/1] via 172.16.1.1, 00:00:06, Serial0/0
R       10.10.40.0/24 [120/1] via 192.168.1.2, 00:00:16, Serial0/1
R       10.10.50.0/24 [120/1] via 192.168.1.2, 00:00:17, Serial0/1
C    192.168.1.0/24 is directly connected, Serial0/1
R2#ping 10.10.30.1
Type escape sequence to abort.
Sending 5, 100-byte ICMP Echos to 10.10.30.1, timeout is 2 seconds:
!!!!!
Success rate is 100 percent (5/5), round-trip min/avg/max = 1/12/24 ms

# Now back over to R1 to check that automatic summarization is turned off (not in effect):
R1#show ip protocols
Routing Protocol is “rip”
Redistributing: rip
Automatic network summarization is not in effect
Maximum path: 4


[output truncated]
```

#exam Es muy importante conocer bien este concepto porque puede ser una via factible de Cisco para poder atraparte durante el examen. 