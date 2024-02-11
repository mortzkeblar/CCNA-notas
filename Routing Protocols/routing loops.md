---
tags:
  - routing
  - dynamic
---

Los routing loops puede provocar castastrofes para la red, estos consumen ancho de banda y recursos causando perdida de información. Además puede tomar tiempo en localizarlo y depurarlo. 
Esto suele suceder principalmente cuando la convergencia de la red es lenta, entonces los routers anuncian routing tables incorrectos. Un router puede perder conexión a la red, pero los demás routers aun no son conscientes del problema. 

 > routing loops can happen quickly

![](_anexos_/Screenshot%20from%202024-01-29%2018-06-30.png)

Eventualmente, la nueva actualización llega a router C, sin embargo, en ese tiempo, router A piensa que `172.20.0.x` esta todavia activa. Entonces, router A informa a Router D que puede encontrar `172.20.0.x ` (via Router C y B) basado en la información anterior al cambio. Ahora los routers B, C, y D creen que Router A tiene una ruta a la red `172.20.0.x` y enrutan el trafico hacia A.  

Cuando router A recibe el trafico, lo dirige hacia router C (basado en la información anterior, actualmente erronea). Aumentando el TTL (Time to live) en cada salto entre routers (255), esto significa que el paquete sera descartado despues de pasar por los routers 255 veces. Este bucle si bien termina, consume ancho de banda, perdida de rendimiento y degrada la convergencia de la red.
