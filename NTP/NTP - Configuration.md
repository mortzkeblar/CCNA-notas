
### Configuración de hora y fecha con NTP
``` bash
Router$ show clock
*10:24:15.913 UTC Thu Dec 28 2023

## Manual set
Router$ clock set 13:50:02 15 Jan 2016
```

#### Server NTP

``` bash
## Router server
Router$ ntp master <STRATUM>      # El parametro STRATUM es opcional

## Router guests
Router(config)$ ntp server <IP_MASTER> 

### Para configurar el server con un dominio
Router(config)$ ip name-server 8.8.8.8
Router(config)$ ntp server ntp.cisco.come
```

``` bash

### Información sobre sincronización cliente - servidor
Router$ ntp associations
```

![](_anexos_/Screenshot%20from%202023-12-28%2007-50-16.png)