---
tags:
  - CCNA
date created: Saturday, October 19th 2024, 11:08:14 pm
date modified: Wednesday, November 20th 2024, 11:33:50 pm
---
El protocolo [[EIGRP]] permite el unequal-cost load balancing. Esto significa que el trafico se distribuya sobre enlaces con diferentes velocidades. 

``` 
R2#show ip protocols
Routing Protocol is eigrp 150
[Output truncated]
Automatic network summarization is not in effect
Maximum path: 4  # default links load balancing across EIGRP 
Routing for Networks:
150.1.1.2/32
```

![[Pasted image 20240905095236.png]]

Para editar la cantidad de rutas consideradas en el load balancing algorithm, puede usar el comando `maximum-paths [1-6]`.

Puede ver la proporción de traffic-sharing entre los enlaces usando `show ip route [network]`, cuando el load balancing tiene equal-cost, los valores de traffic-share se establecen en 1 para todos los enlaces.

``` 
R2#show ip route 172.16.0.0
Routing entry for 172.16.0.0/16
Known via “eigrp 1”, distance 90, metric 409600, type internal
Redistributing via eigrp 1
Last update from 192.168.2.1 on FastEthernet0/1, 00:02:02 ago
Routing Descriptor Blocks:
192.168.2.1, from 192.168.2.1, 00:02:02 ago, via FastEthernet0/1
Route metric is 409600, traffic share count is 1
Total delay is 6000 microseconds, minimum bandwidth is 10000 Kbit
Reliability 255/255, minimum MTU 1500 bytes
Loading 1/255, Hops 1
* 192.168.1.1, from 192.168.1.1, 00:02:02 ago, via FastEthernet0/0
Route metric is 409600, traffic share count is 1
Total delay is 6000 microseconds, minimum bandwidth is 10000 Kbit
Reliability 255/255, minimum MTU 1500 bytes
Loading 1/255, Hops 1
```

> Por defecto [[EIGRP]] realiza equal-cost load balancing, para habilitar unequal-cost load balancing ejecutar `varience [multiplier]`.
- `multiplier` es un numero el cual se multiplica por el enlace con la [[(LEGACY) metric]]a más baja, indica que el load balancing se realiza sobre todos los enlaces que tenga la [[(LEGACY) metric]]a  menor al resultado del número descrito anteriormente. 
\
Por ejemplo, si tenemos dos rutas:
- Un enlace f0/0 con una [[(LEGACY) metric]] de 409600 (por defecto)
- Una enlace f0/1 con una [[(LEGACY) metric]] de 2713600 (un [[(LEGACY) metric]] mayor producto de un bandwidth menor por ejemplo)

Se puede realizar el unequal-cost load balancing ajustando el varience que en este caso, calculamos el multiplier como: $$multiplier=\frac{2713600\ (highest\ metric)}{409600\ (successor\ route\ metric)}=6.625=7$$
Una vez seteado `multiplier 7`, este indica que el load balancing se va a realizar con los enlaces que tengan el valor de la metrica menor a $7\cdot 409600=2867200$, por lo cual f0/1 cumple la condición y pasaria a realizar el load balancing correspondiente. 

```
R2(config)#router eigrp 1
R2(config-router)#variance 7
R2(config-router)#exit
Now, let’s examine the routing table for the 172.16.0.0/16 prefix:
R2#show ip route 172.16.0.0 255.255.0.0
Routing entry for 172.16.0.0/16
Known via “eigrp 1”, distance 90, metric 409600, type internal
Redistributing via eigrp 1
Last update from 192.168.2.1 on FastEthernet0/1, 00:00:27 ago
Routing Descriptor Blocks:
192.168.2.1, from 192.168.2.1, 00:00:27 ago, via FastEthernet0/1
Route metric is 2713600, traffic share count is 3
Total delay is 6000 microseconds, minimum bandwidth is 1000 Kbit
Reliability 255/255, minimum MTU 1500 bytes
Loading 1/255, Hops 1
* 192.168.1.1, from 192.168.1.1, 00:00:27 ago, via FastEthernet0/0
Route metric is 409600, traffic share count is 20
Total delay is 6000 microseconds, minimum bandwidth is 10000 Kbit
Reliability 255/255, minimum MTU 1500 bytes
Loading 1/255, Hops 1
```