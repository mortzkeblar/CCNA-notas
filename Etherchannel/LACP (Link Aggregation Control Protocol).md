Se utiliza el protocolo LACP para negociar y mantener el etherchannel entre dos switches cisco o de otro vendor. Incluso permite levantar un etherchannel entre un switch Cisco y un servidor de virtualización (hypervisor).

![](_anexos_/Screenshot%20from%202024-01-04%2017-26-30.png)

``` bash
SW1(config)$ interface range g0/2-3
SW1(config-if-range)$ channel-group 1 mode active     
SW1(config-if-range)$ exit

SW1(config)$ interface range g0/2-3
SW1(config-if-range)$ channel-group 1 mode passive 
SW1(config-if-range)$ exit
```

- _Active_, configura el puerto en modo de negociacion activa. Intentara formar un etherchannel enviando mensajes LACP
- _Passive_, configura el puerto en modo de negociacion pasiva. Respondera mensajes LACP pero no iniciara negociacion del etherchannel