En este lab vamos a crear una red que funcione bajo [RIPv2](RIPv2.md).

Video referencia: https://www.youtube.com/watch?v=5hIXd4WfB7w&list=PL2A7l6PiV52esSwosIAO86zf0RGe2pjTZ&index=26

![](_anexos_/Screenshot%20from%202023-12-27%2021-21-51.png)

#### Configuración de RIPv2
``` bash
R1(config)$ router rip    # up 520 UDP listening
# Indicamos cual es la red que tenemos conectada en cada una de las interfaces.
## Podemos indicar la subred de la interfaz
## Tambien el bloque classfull que corresponda a la subred
R1(config)$ network 192.168.0.128
R1(config)$ network 192.168.0.0
R1(config)$ network 192.168.0.8
```

No es estrictamente necesario especificar las tres IPs, con la IP de classfull deberia ser suficiente.
``` bash
R1(config-router)$ do show run | section router
router rip
 network 192.168.0.0

## Entonces podemos ejecutar el mismo comando en R2 y R3
R1(config-router)$ network 192.168.0.0
```

Para verificar que este configurado correctamente:
``` bash
R3(config-router)$ do show ip protocols
...
  Default version control: send version 1, receive any version
...
```

Se puede ver que se esta usando la versión 1 de RIP. Para cambiar a la versión 2.
``` bash
R3(config)$ router rip
R3(config-router)$ version 2

## Debemos hacer esto porque tenemos diversas subredes con VLSM. Cuestión que la versión 1 no soporta.
```


> Importante: es recomendable siempre deshabilitar la auto-sumarización al trabajar en RIPv2. Ver [auto summarization](auto%20summarization.md)

Para filtrar las rutas configuradas por `RIP`. 
``` bash
R1$ show ip route rip
Codes: L - local, C - connected, S - static, R - RIP, M - mobile, B - BGP
       D - EIGRP, EX - EIGRP external, O - OSPF, IA - OSPF inter area 
       N1 - OSPF NSSA external type 1, N2 - OSPF NSSA external type 2
       E1 - OSPF external type 1, E2 - OSPF external type 2
       i - IS-IS, su - IS-IS summary, L1 - IS-IS level-1, L2 - IS-IS level-2
       ia - IS-IS inter area, * - candidate default, U - per-user static route
       o - ODR, P - periodic downloaded static route, H - NHRP, l - LISP
       a - application route
       + - replicated route, % - next hop override, p - overrides from PfR

Gateway of last resort is not set

      192.168.0.0/24 is variably subnetted, 9 subnets, 3 masks
R        192.168.0.4/30 [120/1] via 192.168.0.10, 00:00:04, GigabitEthernet0/1
                        [120/1] via 192.168.0.2, 00:00:10, GigabitEthernet0/0
R        192.168.0.136/29 
           [120/1] via 192.168.0.2, 00:00:10, GigabitEthernet0/0
R        192.168.0.144/29 
           [120/1] via 192.168.0.10, 00:00:04, GigabitEthernet0/1

```
Podemos ver que las IPs de `Loopback0` ya son accesibles en la versión 2 de `RIP`

Podemos verificar que ruta toma un paquete en su recorrido con :
``` bash
R1$ traceroute 192.168.0.129
```

Para poder forzar la comprobación desde otra interfaz (el que nos interesa en este caso es el que esta conectado a `Loopback0`) hacia `Lo0` del otro router ejecutamos.
``` bash
R2$ ping 192.168.0.129 source lo0
```

Para poder desactivar el envio de mensajes de enrutamiento de `RIPv2`. 
``` bash
R1$ router rip
R1(config-router)$ passive-interface loopback 0
R1(config-router)$ passive-interface default # En caso de querer deshabilitar todas las interfaces.

R1(config-router)$ do show run | section router rip
router rip
 version 2
 passive-interface Loopback0
 network 192.168.0.0
```

Para verificar que no se este enviando por `loopback0` mensajes podemos hacer debug.




