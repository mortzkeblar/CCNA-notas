Cuando se usa este comando, las rutas hacia que pasan por el router son considerar como gateway of last resort. Si existe una ruta estatica, solo afectara al trafico que salga del mismo router. 

Con el comando ip default-network, cada router conocerá la red por defecto a través de un protocolo de enrutamiento y enviará el tráfico a esa dirección si no hay otra ruta en la tabla de enrutamiento (en lugar de descartarlo).

``` bash
ip default-network <IP> 
```

> No funciona con OSPF, usar `default-information originate`

`ip default-network ` permite a los routers reenviar paquetes cuando no existe otra ruta y la red por defecto se envia a los otros routers. 


![](_anexos_/Screenshot%20from%202024-01-29%2015-06-35.png)



