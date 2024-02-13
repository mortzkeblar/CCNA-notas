---
tags:
  - NAT
  
  
---

Este tipo de NAT es uan variación del PAT tradicional pero se agrega un grupo de IP publicas para hacer la traducción. Esto permite tener más $2^{16}$ conexiones simultaneas (que es lo que permite una sola IP).

![](../_anexos_/Screenshot%20from%202023-12-31%2018-36-22.png)

``` bash
Router(config)$ ip nat pool RANGO_PUBLICO 200.1.1.1 200.1.1.4 netmask 255.255.255.248
Router(config)$ access-list 1 permit 192.168.0.0 0.0.0.255
Router(config)$ ip nat inside source list 1 pool RANGO_PUBLICO overload # Esta es la unica linea diferencial con NAT dinamico

Router(config)$ interface f0/0
Router(config-if)$ ip nat inside
Router(config-if)$ exit
Router(config)$ interface f0/1
Router(config-if)$ ip nat outside 
```

> Situación: si ejecutas `netstat -na` puedes ver la cantidad de conexiones por puerto van generando las apps en tu dispositivo, si consideramos que cada hosts generada aprox 50 conexiones por puerto. Entonces $2^{16}/50$ nos da un aproximados de 1310 hosts para que se puedan conectar de forma simultanea. 
> Si tenemos los $2^{16}$ puertos ocupados en ese momento. Si abrimos una nueva conexión, no va a poder conectarse hasta que se libere un nuevo puerto.
> Esto es lo que viene a arreglar justamente el _overload dynamic NAT._