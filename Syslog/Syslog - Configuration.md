---
tags:
  - SYSLOG
  - CCNA
date created: Saturday, October 19th 2024, 11:08:14 pm
date modified: Wednesday, November 20th 2024, 11:33:50 pm
---

``` bash
R1(config)$ service timestamp log
R1(config)$ logging console disable
R1(config)$ logging 192.168.76.2
R1(config)$ logging trap 7
R1$ show logging 
```

_Recomendaciones:_
- Habilitar el registro de hora y fecha (timestamp) en los mensajes de log
- Deshabilitar el registro de consola, ya que podría causar aumento de uso de CPU
- Habilitar [NTP](NTP.md) (mucho muy importante)
- Utilizar con precaución los niveles de log, porque podrían causar un aumento en el tráfico syslog en una red saturada