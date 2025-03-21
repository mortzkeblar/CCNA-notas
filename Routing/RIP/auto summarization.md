---
tags:
  - routing
  - RIP
  - dynamic
  - CCNA
date created: Saturday, October 19th 2024, 11:08:14 pm
date modified: Wednesday, November 20th 2024, 11:33:50 pm
---

Esta es una función de [RIPv2](RIPv2.md) que 'resume' automaticamente las redes en los limites de las clases [classful](classful.md). 
- P. ej. si se anuncia la red `172.6.1.0/26`, cruza por la red `10.0.0.0/8`. La ruta `172` se resumira automaticamente a `172.6.1.0/16` (Clase B). 


Esta opción generalmente viene activada por defecto, se puede comprobar con el comando `show ip protocols`.  

Se aconseja desactivar el auto summarization porque suele ser el causante de algunos problemas. 

``` bash
R2(config-router)$ router rip
R2(config-router)$ no auto-summary
```