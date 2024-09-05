[[EIGRP]] usa una [[metric]] compuesta para determinar el mejor camino entre A y B. Esta composición se basa en cuatro atributos principales:
- Bandwidth
- Delay 
- Load 
- Reliability 
Estos atributos se usan en una formula para determinar la metrica EIGRP usada. Estos valores pueden ser modificados, los cuales se les conoce como _K values_. 

### bandwidth 
Esta es expresada en _Kilobits_. EIGRP toma el bandwidth por defecto en la interface, la cual puede ser cambiada con el comando `bandwidth`

``` bash
Serial 0/0 is up, line protocol is up
Hardware is MCI Serial
Internet address is 192.168.10.203, subnet mask is 255.255.255.0
MTU 1500 bytes, BW 1544 Kbit, DLY 20000 usec, rely 255/255, load 1/255
```

### delay 
Esta es expresada en _microsegundos_, puede ser ajustada manualmente con el comando `delay`. Cisco recomienda usar el comando `delay` en vez de `bandwidth` si se desea influir sobre las decisiones de enrutamiento [[EIGRP]]. 

> Modificar estos valores no afectan al rendimiento de la interface, solo afecta a la metrica de enrutamiento.

### reliability 
Es un rango dinamico de numeros desde el 1 hasta 255. El valor 255 significa que el enlace es el más confiable. 

### load 
En un rango que va desde 1 hasta 255. representa la carga de salida de una interface. El valor 1 representa el minimo de carga y el 255 representa una carga del 100%. 

> En el calculo tambien se comtempla el valor MTU para el envio de paquetes 

La composición de la metrica EIGRP es usada calculando la siguiente formula:
$$[(K_{1}\cdot bandwidth\ + \frac{K_{2}\cdot bandwidth}{256 - load}+K_{3}\cdot delay)\ \cdot \frac{K_{5}}{K_{4}+reliability}]\cdot 256$$

Por defecto los valores las variables $K$ son $K_{1}=K_{3}=1$ y $K_{2}=K_{4}=K_{5}=0$, entonces por simplificación se obtiene:
$$metric=(bandwidth +delay)\cdot 256$$
Sin embargo, el calculo [[EIGRP]] no solo utiliza los valores de bandwidth / delay de la propia interface. EIGRP usa el bandwidth minimo de la ruta (lo cual tiene sentido pues ahi esta el bottleneck) y la suma de los delays de todos los enlaces en la ruta. Estos valores se obtiene usando las siguientes formulas: 
$$bandwidth=\frac{256*107}{minimun\ bandwidth\ in\ on\ path\ (kilobits)}$$
$$delay=256\cdot \text{sum of delays (expressed in 10s of microseconds)}$$

Entonces, el valor default de la metrica EIGRP se puede expresar como:
$$metric = [\frac{10^{7}}{\text{least bandwidth on path}}+\text{sum of all delays}]\cdot 256$$

El calculo de esta [[metric]] puede ajustarse variando sobre los _k values_, con el comando `metric weights [tos] k1 k2 k3 k4 k5` (`tos` representa el type of service, normalmente esta seteado en `0`). Los _k values_ puede tomar valores de entre 0 a 255. 

``` bash
Router#show ip protocols
Routing Protocol is eigrp 150
Outgoing update filter list for all interfaces is not set
Incoming update filter list for all interfaces is not set
Default networks flagged in outgoing updates
Default networks accepted from incoming updates
EIGRP metric weight K1=1, K2=0, K3=1, K4=0, K5=0
EIGRP maximum hopcount 100
EIGRP maximum metric variance 1
Redistributing: eigrp 150
EIGRP NSF-aware route hold timer is 240s
Automatic network summarization is not in effect
Maximum path: 4
Routing for Networks:
192.168.1.0
Routing Information Sources:
Gateway Distance Last Update
192.168.1.3 90 00:00:15
Distance: internal 90 external 170
```

> Es importante recordar que para que se pueda establecer un neighbor relationship entre dos routers, deben coincidir los _k values_ entre ellos. Por lo cual al modificar los _k values_ de uno de los routers, se debe garantizar que ese cambio se produzca tambien para todo los routers EIGRP dentro del [[autonomous system]]. 