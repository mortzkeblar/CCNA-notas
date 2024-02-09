---
tags:
  - ETHERCHANNEL
  - concept
  - configuration
---

Se utiliza el protocolo PAgP para negociar y mantener el etherchannel entre dos switches Cisco unicamente.

![](_anexos_/Screenshot%20from%202024-01-04%2017-15-46.png)

``` bash

SW1(config)$ interface range g0/2-3
SW1(config-if-range)$ channel-group 1 desirable      
SW1(config-if-range)$ exit

SW2(config)$ interface range g0/2-3
SW2(config-if-range)$ channel-group 1 mode auto      
SW2(config-if-range)$ exit
```

- _Desirable_, configura el puerto en modo de negociacion activa. Activamente intentara formar un etherchannel enviando mensaje PAgP
- _Auto_, configura un puerto en modo de negociacion pasiva. Respondera mensajes PAgP pero no iniciara negociacion del etherchannel