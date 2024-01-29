# gateway of last resort 
Este recurso sirva para desviar un paquete que no ha podido encontrar  ninguna ruta en la [administrative distances](administrative%20distances.md) por donde seguir. Sin esta configuración, el paquete seria descartado. 
Esto puede habilitarse a través del default-gateway o bien enrutandolo hacia `0.0.0.0`.
``` bash
ip default-gateway 
ip route 0.0.0.0 0.0.0.0
```


