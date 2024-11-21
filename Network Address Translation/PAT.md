---
tags:
  - NAT
  - CCNA
date created: Saturday, October 19th 2024, 11:08:14 pm
date modified: Wednesday, November 20th 2024, 11:33:50 pm
---

El tipo de NAT más ampliamente utilizado hoy en las organizaciones. Consiste en permitir que múltiples IP privadas se conecten a internet utilizando una sola IP (overloading). Aquí la traducción se hace en base a puertos.

``` txt
En este caso podemos multiples IPs privadas con una unica IP publica (en el ejemplo se toma la IP de la interfaz 200.1.1.1/29).
- Esto es comun en router domesticos tipo SOHO (small office, home office)
```

![](../_anexos_/Screenshot%20from%202023-12-31%2017-57-51.png)

Cuando un host quiera salir por internet, llega al router el cual lo agrega en un tabla `stateful` donde va a asociarlo con un puerto de la IP publica. Si tenemos en cuenta que la cantidad de puertos disponibles son $2^{16}$. Esa es la cantidad maxima de IPs que podemos conectar simultáneamente hacia internet por PAT. 

``` bash
## Creamos una access-list para seleccionar los hosts que queremos exponer con la NAT
Router(config)$ access-list 1 permit 192.168.0.0. 0.0.0.255
Router(config)$ ip nat inside source list 1 int f0/1 overload
Router(config)$ interface f0/0
Router(config-if)$ ip nat inside
Router(config-if)$ exit
Router(config)$ interface f0/1
Router(config-if)$ ip nat outside
```