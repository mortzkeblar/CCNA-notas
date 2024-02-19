---
tags:
  - routing
---

Classless inter-domain routing es un metodo de asignamiento de IPs públicas, introducido en el '93 por la IETF con los siguientes objetivos.
- Tratar de resolver el problema de agotamiento de direcciones IPv4.
- Relantizar el crecimiento de la tablas de enrutamiento en los routers de internet.

Ademas se trato de solventar el problema de desperdicio de direcciones IP con los metodos [classful](classful.md) de direccionamiento. 

[CIDR]() eliminó los requisitos fijos de `/8`, `/16` y `/24` para cada clase y permitió dividirla en todo el rango unicast (primer octeto 0, 225), lo que ahora llamamos [subnetting](../labs/NetWarriors/subnetting.md). 

Para poder simplificar la notación, se adopto la _slash notation_. P. ej. la subnet `255.255.255.128` era equivalente a `/25`.

![](_anexos_/2018-06-08_11-57-24-b43b39665963f818d49609459d652f04.png)