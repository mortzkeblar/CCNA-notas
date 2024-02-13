---
tags:
  - OSPF
  - dynamic
  - routing
  - lab
---
Temas a tratar:
- [OSPF](OSPF.md) 

``` bash
show ip ip int brief
show ip ospf neighbor 
show ip route ospf 
show ip ospf database
```

![](_anexos_/Screenshot%20from%202024-02-12%2013-13-00.png)

``` bash
# R1

en
conf t
hostname R1

int g0/0
ip add 192.168.12.1 255.255.255.0
no sh

int g0/1
ip add 192.168.14.1 255.255.255.0 
no sh
int 

int g0/2
ip add 192.168.1.1 255.255.255.0 
no sh

router ospf 1

int g0/0
ip ospf 1 area 0

int g0/1
ip ospf 1 area 0

int g0/2
ip ospf 1 area 0

# R2 

en
conf t
hostname R2

int g0/0
ip add 192.168.12.2 255.255.255.0
no sh

int g0/1
ip add 192.168.23.1 255.255.255.0 
no sh

router ospf 1

int g0/0
ip ospf 1 area 0

int g0/1
ip ospf 1 area 0

# R3

en
conf t
hostname R3

int g0/0
ip add 192.168.23.2 255.255.255.0
no sh

int g0/1
ip add 192.168.34.1 255.255.255.0 
no sh
int 

int g0/2
ip add 192.168.2.1 255.255.255.0 
no sh

router ospf 1

int range g0/0-2
ip ospf 1 area 0
exit

# R4

en
conf t
hostname R4

int g0/0
ip add 192.168.34.2 255.255.255.0
no sh

int g0/1
ip add 192.168.14.2 255.255.255.0 
no sh

router ospf 1

int g0/0
ip ospf 1 area 0

int g0/1
ip ospf 1 area 0

```

#exam En este lab se habilito OSPF a través de interfaces y no de ip con `network`. Ambos cumplen la misma función pero puede ser que en practicas como Boson (comprobado) solo deseen una forma de hacer las cosas, tener cuidado. Sobretodo en la formulación de la pregunta/practica, por ejemplo: habilitar OSPF para anunciar subredes especificas (no en general). 