---
tags:
  - CCNA
date created: Saturday, October 19th 2024, 11:08:14 pm
date modified: Wednesday, November 20th 2024, 11:33:50 pm
---
Este suele ser una servidor http basico para pruebas. 

``` bash
# Habilitar el servidor http en el Router
R1(config)$ ip http server
```

``` bash
# Para habilitar la autenticación
R1(config)$ username <USER> privilege 15 password <PASSWORD>
R1(config)$ ip http authentication local
```

> Recomentación, si buscamos un servicio http usando IPv6, los navagadores suele comportarse de manera irregular. Usar esta solución si esto ocurre:
> De `http://2000:3::F` a `http://[2000:3::F]`