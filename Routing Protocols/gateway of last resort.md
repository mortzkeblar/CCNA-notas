Este recurso sirva para desviar un paquete que no ha podido encontrar  ninguna ruta en la [administrative distance](administrative%20distance.md) por donde seguir. Sin esta configuración, el paquete seria descartado. 
Esto puede habilitarse a través de [IP default-gateway](IP%20default-gateway.md)  o bien enrutandolo hacia `0.0.0.0`.
``` bash
ip default-gateway 
ip route 0.0.0.0 0.0.0.0
```



