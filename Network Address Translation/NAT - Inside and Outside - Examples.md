---
tags:
  - NAT
  - CCNA
date created: Saturday, October 19th 2024, 11:08:14 pm
date modified: Wednesday, November 20th 2024, 11:33:50 pm
---

_Para entender mejor el concepto de inside/outside ver: [NAT - Inside and Outside](NAT%20-%20Inside%20and%20Outside.md)_

#### Example 1

![](../_anexos_/Screenshot%20from%202024-01-01%2008-51-30.png)

``` bash
## En este caso, el trafico siempre va desde la LAN hacia internet
### En otras palabras, desde inside hasta outside

R1(config-if)$ int s0/0
R1(config-if)$ ip address 200.1.1.1 255.255.255.0
R1(config-if)$ ip nat outside

R1(config-if)$ int f0/1
R1(config-if)$ ip address 192.168.0.1 255.255.255.0
R1(config-if)$ ip nat inside
```

#### Example 2

![](../_anexos_/Screenshot%20from%202024-01-01%2008-55-59.png)

``` bash
# Aca como estamos trabajando con interfaces virtuales, la instruccion solo enciende la interfaz fisica pero no lo configura en ip nat porque no tiene asignada una IP. Recordad que NAT trabaja sobre L3, entonces asignamos ip nat sobre las interfaces virtuales.

Router(config-if)$ int s0/0
Router(config-if)$ ip address 200.1.1.1 255.255.255.0
Router(config-if)$ ip nat outside

# Configuración red inside
Router(config-if)$ int f0/0
Router(config-if)$ no shut

## Subinterfaces de f0/0
Router(config-if)$ int f0/1.1
Router(config-if)$ ip address 192.168.1.1 255.255.255.0
Router(config-if)$ ip nat inside
Router(config-if)$ int f0/1.2 
Router(config-if)$ ip address 192.168.2.1 255.255.255.0
Router(config-if)$ ip nat inside
Router(config-if)$ int f0/1.3
Router(config-if)$ ip address 192.168.3.1 255.255.255.0
Router(config-if)$ ip nat inside
```