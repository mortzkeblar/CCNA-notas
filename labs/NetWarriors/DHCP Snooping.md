---
tags:
  - DHCP
  - lab
  - CCNA
---


![](_anexos_/Screenshot%20from%202024-01-07%2003-02-58.png)

``` bash
# Desactivar interfaces no utilizadas
SW(config)$ int range f0/4-24
SW(config)$ shutdown
```

``` bash
# DHCP seguridad
SW(config)$ ip dhcp snooping
SW(config)$ ip dhcp snooping vlan 2 3
SW(config)$ int f0/1
SW(config-if)$ ip dhcp snooping trust

## Limitamos la velocidad de solicitudes, en el switch que esta conectado directamente al servidor DHCP
SW(config)$ int f0/2
SW(config-if)$ ip dhcp snooping limit rate 5
SW(config-if)$ exit
SW(config)$ ip dhcp snooping vlan 2 3
SW(config)$ int f0/1
SW(config-if)$ ip dhcp snooping trust

```


##### References
- https://www.youtube.com/watch?v=2dstdqOfVow&list=PLoqM5eIpDpUEhtUNKuOd-sGll7NrvlwD7&index=13
