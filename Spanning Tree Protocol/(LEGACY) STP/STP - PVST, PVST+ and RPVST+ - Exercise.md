---
tags:
  - STP
  - lab
  - CCNA
---

_Para ver el ejercicio y resolución: https://youtu.be/dcTzBa9A43g?list=PL2A7l6PiV52esSwosIAO86zf0RGe2pjTZ&t=270_

``` bash
# Configuracion inicial
ALS1(config)$  int f0/1,f0/3
ALS1(config-if)$ switch mode trunk

## Replicar en DLS1 y DLS2

### Para ver el estado de la interface y trunk
ALS1(config)$ do show interface trunk

## Una vez hecho esto STP ya debio actuar bloqueando los caminos ciclicos
```


``` bash
# Solucion Nro. 1

## Para ver SPT segun el VLAN
ALSA1$ show spanning-tree vlan 10
...
This bridge is the root
...

## Cambiamos la prioridad a DLS1 para forzar a que sea el nuevo root
DLS1(config)$ spanning-tree vlan 10 priority 4096
```

``` bash
# Solucion Nro. 2

## Asignamos a DLS2 como root secundario, el comando lo que hace es cambiar la prioridad a un valor que le permita tener root en caso de que el primer RB falle

DLS2(config)$ spanning-tree vlan 10 root secundary
```

``` bash
# Solucion Nro. 3

# Este comando puedo provocar un proceso de reconvergencia, por lo cual no se recomienda usarlo en prod.

DLS2(config)$ spanning-tree mode rapid-pvst
```

_Ver: [STP](Project/Networking/CCNA-notas/Spanning%20Tree%20Protocol/(LEGACY)%20STP/STP.md)_