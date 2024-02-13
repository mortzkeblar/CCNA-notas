---
tags:
  - TS
  
---

![](_anexos_/Screenshot%20from%202024-01-02%2003-39-06.png)

> Recomendación: no tener el proceso `debug` corriendo demasiado tiempo, dependiendo el equipo puede que lo llegue a sobrecargar y dejarlo temporalmente fuera de servicio.

``` bash
# Habilitar la salida de debug en terminales remotas, por defecto los mensajes de debug se van a la consola serial del dispositivo
R1$ terminal monitor
```

``` bash
R1$ debug ip icmp
ICMP packet debugging is on
....
```

``` bash
# RIPv2
R1$ debug ip rip

## NOTA: rip envia mensajes desde sus interfaces cada 30 segs.
```

``` bash
# NTP
R1$ debug ntp all

R1$ show ntp status
```

``` bash
# ACL Troubleshooting
R1$ show access-list
```

``` bash
# NAT
R1$ debug ip nat
```

``` bash
# Para deshabilitar cualquier tipo de debug
R1$ undebug all
```