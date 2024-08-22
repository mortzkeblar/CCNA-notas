---
tags:
  - routing
  - dynamic
  - problem
  - CCNA
---

Esto siempre depende de si las ventajas superan las desventajas, pero en general los dos principales problemas al usar un protocolo como [OSPF](OSPF.md) son:
- Alto uso de CPU
- Uso del ancho de banda

## cpu and memory requirements
Cuando se corre [OSPF](OSPF.md) se consumen una gran cantidad de ciclos de CPU, esto puede relentizar otros procesos en paralelo corriendo en el router porque recordemos que tiene una CPU limitada. 

En general un [link state protocols](link%20state%20protocols.md) suele consumir más memoria y potencia de procesamiento que un [distance vector protocols](distance%20vector%20protocols.md).

Como contrapartida, estos consumos más altos proporcionan una convergencia más rápida y es más flexible y potente a la hora de configurar ciertos parámetros. 

``` bash
# Carga del CPU en intervalos de 60 segs

RouterA#show processes
CPU utilization for five seconds:4%/0%; one minute: 2%;five minutes: 1%
PID Runtime(ms)Invoked uSecs 5Sec 1Min 5Min TTY Process
1 820 898 913 0.00% 0.03% 0.05% 0 Load Meter
2 16236 1672 9710 4.33% 1.18% 0.61% 0 Exec

# Se puede agregar el argumento cpu para tener intervalos de 5 segs, 1 min y 5 mins 

RouterA#show processes run
```

## bandwidth utilization 
Una vez se produce la convergencia en la red, el ancho de banda que require un [link state protocols](link%20state%20protocols.md) es bajo. Cuando se habilita para que opere este, la red se inunda de paquetes LSA que se mantienen hasta que se produce la convergencia total, si tiene un ancho de banda limitado, esto puede saturar la red por un tiempo.