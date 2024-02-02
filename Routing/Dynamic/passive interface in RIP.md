_Ver: [passive interface](passive%20interface.md) _

En [RIP](../../../../../RIP.md), una interface pasiva recibira actualizaciones de enrutamiento pero no enviará ninguna. 

Si se trata de un gran número de interfaces para agregar, se puede usar el `default` para hacer pasivas todas las interfaces.

``` bash
_R1(config-router)#passive-interface ?_
_FastEthernet       IEEE 802.3_
_default            Suppress routing updates on all interfaces_
[output truncated]
```