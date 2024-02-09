---
tags:
  - ETHERCHANNEL
  - lab
---

> Para ver la resolución del video ir a: https://youtu.be/pha803AApb4?list=PL2A7l6PiV52esSwosIAO86zf0RGe2pjTZ&t=1742


![](_anexos_/Screenshot%20from%202024-01-04%2018-36-32.png)
## Solución
``` bash
# Solucion 1 y 2 (LACP)

# Para ver a los sw vecinos conectados
S1$ show cdp neighbors

S1(config)$ interface range f0/1-2
S1(config-if-range)$ channel-group 2 mode active # LACP
S1(config)$ show etherchannel summary  # Lista de etherchannels creados

S3(config)$ int range f0/1-2
S3(config-if-range)$ channel-group 1 mode passive
```


``` bash
# Configuramos el etherchannel con el trunk mode

## En este caso trabajamos con la interfaz virtual exclusivamente
S1(config)$ int po2    # El nombre po2 se obtiene del etherchannel summary
S1(config-if)$ swhitchport mode trunk # Esto nos deberia generar una advertencia/error por el modo "Auto" en trunk
S1(config-if)$ switchport trunk encapsulation dot1q
S1(config-if)$ switchport mode trunk
S1(config-if)$ show interfaces trunk # Para ver que la interfaz use trunk

## El otro switch tambien va a configurarse automaticamente en modo trunk, esto se debe a DTP (Dynamic Trunking Protocol)

```

``` bash
# Solucion 3 (PAgP)
S2(config-if)$ int ranger f0/1-2
S2(config-if-range)$ channel-group 2 mode desirable # No importa en que lado se use desirable o auto

S3(config-if)$ int range f0/3-4
S2(config-if)$ channel-group 2 mode auto 

## Tambien configurar aca el trunk mode
```

``` bash
# Solucion 4
S1$ show cdp neigh

S1(config)$ int ran f0/3-4
S1(config-if-range)$ channel-group 1 mode on

S2(config)$ int range f0/3-4
S2(config-if-range)$ channel-group 1  mode on

## Como el etherchannel esta configurado manualmente no tiene ningun protocolo de negociación

S2(config-if)$ int po 1
S2(config-if)$ no switchport

## En caso de que arroje el error 'Not a convertible port'. Debemos convertir las int fisicas en modo router y luego convertir el port channel
S2(config)$ int range f0/3-4
S2(config-if-range)$ no sw
S2(config-if-range)$ int po 1
S2(config-if-range)$ no sw

### Realizar lo mismo en S1

# Asignamos un IP address a los port channel
S1(config-if)$ int po1
S1(config-if)$ ip address 192.168.0.1 255.255.255.252

S2(config-if)$ int po1
S2(config-if)$ ip address 192.168.0.2 255.255.255.252

```

