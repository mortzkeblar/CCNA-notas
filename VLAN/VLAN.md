---
tags:
  - VLAN
  
---


Las _Virtual Local Area Network_ (o VLAN) son agrupaciones lógicas de dispositivos en un mismo dominio [broadcast] (para entender esto ultimo ver como funciona un [switch]). 

Esto es bastante util para _dividir_ el switch en diferentes segmentos LAN, porque con VLAN podemos crear subredes con sus propios dominios [broadcast] y definir que que dispositivos queremos que comuniquen a través de esa subred. Considerar que por defecto un switch solo tiene un dominio broadcast para enviar todo el trafico de la red y ese es el número `1`, también llamado [VLAN Native](VLAN%20Native.md).
- Las VLAN aumentan la cantidad de dominios broadcast al tiempo que reducen su tamaño.
- Las VLAN reducen los riesgos de seguridad al reducir la cantidad de hosts que reciben copias de las tramas que inundan los switches.
- Puede mantener los hosts que contienen datos confidenciales en una VLAN separada para mejorar la seguridad.
- Puede crear diseños de red más flexibles que agrupen a los usuarios por departamento en lugar de por ubicación física.
- Los cambios de red se logran con facilidad simplemente configurando un puerto en la VLAN adecuada.


> Si queremos comunicarnos con hosts de diferentes VLAN, necesitamos un router 
> #TODO agregar entrada sobre esto



### References
- https://www.ccnablog.com/vlans-part-i/
- https://study-ccna.com/what-is-a-vlan/