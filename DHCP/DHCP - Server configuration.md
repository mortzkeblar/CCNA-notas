---
tags:
  - DHCP
  - CCNA
---

Este apartado cubre la configuración de un [DHCP](DHCP.md) server en un router cisco.

``` bash
# Configuración de la interfaz g0/0
R1(config)$ int g0/0
R1(config-if)$ ip address 192.168.0.1 255.255.255.0
R1(config-if)$ no shutdown

# Creacion del DHCP pool con nombre "DCHP-POOL-1"
R1(config)$ ip dhcp pool DHCP-POOL-1
R1(dhcp-config)$ default-router 192.168.0.1
R1(dhcp-config)$ dns-server 8.8.8.8           # usamos el DNS de Google
R1(dhcp-config)$ network 192.168.0.0 255.255.255.0
R1(dhcp-config)$ exit

# Opcional (podemos excluir IPs del DHCP Pool)
R1(config)$ ip dhcp excluded-address 192.168.0.1 192.168.0.99
R1(config)$ exit  
```