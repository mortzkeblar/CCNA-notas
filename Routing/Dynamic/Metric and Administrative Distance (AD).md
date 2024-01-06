Los valores del Routing Table Entry provienen de `show ip route`. 

![](_anexos_/Screenshot%20from%202023-12-27%2016-43-38.png)

- _Distancia administrativa:_ es un numero (0 - 255), especifica el nivel de confiabilidad del metodo de enrutamiento, `más bajo = mejor`.

#### Valores importantes sobre la distancia por defecto
| Route Source                       | Default Distance |
| ---------------------------------- | ---------------- |
| Connected interface                | 0                |
| Static route out an interface      | 0                |
| Static route to a next-hop address | 1                |
| EIGRP summary route                | 5                |
| Internel EIGRP                     | 90               |
| OSPF                               | 110              |
| RIPv1, RIPv2                       | 120              |
| External EIGRP                     | 170              |
| Unknown                                   | 255                 |
![](_anexos_/Screenshot%20from%202023-12-27%2016-57-08.png)

En este caso, el route debe tomar la decisión de por donde ir hacia 20.0.0.0, esto se define a través de la tabla de distancia. La mejor decisión es el valor más bajo, en este caso, EIGRP. 
- La `routing-table` solo contiene la información de la mejor ruta. 

### Diferencia con `métrica`
La diferencia reside esencialmente en que la métrica rige cuando se aprende el prefijo a través de una unico metodo de enrutamiento. 

![](_anexos_/Screenshot%20from%202023-12-27%2017-04-58.png)

En este caso, todos los routers funcionan bajo el protocolo RIP con los mismos valores de `default distance`. Entonces la decisión se basa en los diversos metodos que aplica cada protocolo, RIP por ejemplo usa `Hop Count`, OSPF usa `Cost(Band Width)`, etc.

Ver: [Best routing path selection criteria](Routing/Dynamic/Best%20routing%20path%20selection%20criteria.md)