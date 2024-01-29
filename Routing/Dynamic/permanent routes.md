# permanent routes
Una ruta permanente puede omitir la regla de que la interface debe estar enabled y debe exister un next-hop IP alcanzable (reachable). 
Esta ruta permanece en la tabla de enrutamiento a pesar de no cumplir con alguno o los dos requisitos anteriores.
> Un motivo de uso es no querer que ninguna ruta vaya hacia los vecinos BGP superiores

``` bash
_ip route 172.16.0.0 255.255.0.0 10.1.1.1 permanent_
```



