---
tags:
  - VLAN
  - concept
---

## Ethernet Frame

![](_anexos_/Screenshot%20from%202024-01-02%2004-10-40.png)

- Como el campo VLAN ID tiene 12 bits de espacio, esto nos permite tener $2^{12}$ VLANs, es decir, `4096` (Desde el 0 hasta el 4095).
- Regularmente se puede usar las VLAN desde el 1 hasta el 1000
	- Si necesitamos VLANs más alla de ese número, necesitamos de extended VLANs

Desde la versión 12.4(15)T de IOS de Cisco que se puede utilizar los VLAN IDs en el rango entre `1006` y `4094`.

![](_anexos_/Screenshot%20from%202024-01-02%2004-15-00.png)

> Importante:
> - VLANs normales se almacenan en la flash dentro del archivo **vlan.dat**.
> - Extended VLANs se almacenan en la configuración de ejecución (`running-config`)
> - No se pueden crear VLAN extendidas cuando la opción de MAC address reducidas esta desactiva (**spanning-tree extend system-id**)
> - Solamente soportadas en VTPv3

_Ver: [VLAN - Extended Syntax](VLAN%20-%20Extended%20Syntax.md)_