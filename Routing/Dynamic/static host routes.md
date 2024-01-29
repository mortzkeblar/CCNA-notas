# static host routes
A diferencia de un subnet. Esta es una dirección de un unico host, quiere decir que tiene una mascara `255.255.255.255` o `/32` en la ruta. El router usa la [longest prefix matching](longest%20prefix%20matching.md) si es que hay otra entrada en la tabla de enrutamiento coincidente.

``` bash# static host routes
A diferencia de un subnet. Esta es una dirección de un unico host, quiere decir que tiene una mascara `255.255.255.255` o `/32` en la ruta. El router usa la [longest prefix matching](longest%20prefix%20matching.md) si es que hay otra entrada en la tabla de enrutamiento coincidente.

``` bash# static host routes
A diferencia de un subnet. Esta es una dirección de un unico host, quiere decir que tiene una mascara `255.255.255.255` o `/32` en la ruta. El router usa la [longest prefix matching](longest%20prefix%20matching.md) si es que hay otra entrada en la tabla de enrutamiento coincidente.

``` bash# static host routes
A diferencia de un subnet. Esta es una dirección de un unico host, quiere decir que tiene una mascara `255.255.255.255` o `/32` en la ruta. El router usa la [longest prefix matching](longest%20prefix%20matching.md) si es que hay otra entrada en la tabla de enrutamiento coincidente.

``` bash

En este ejemplo configuramos una static host route, la primera ruta es una red y la segunda es un host especifico en la misma red (que matchea gracias al longest prefix matching)
_ip route 172.16.0.0 255.255.0.0 10.1.1.1_

_ip route 172.16.0.10 255.255.255.255 192.168.1.1_
```

![](_anexos_/13-9-scaled.jpg)