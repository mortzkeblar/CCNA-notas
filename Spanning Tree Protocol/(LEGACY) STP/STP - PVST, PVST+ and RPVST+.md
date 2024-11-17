---
tags:
  - STP
  - CCNA
---


![](../../_anexos_/Screenshot%20from%202024-01-04%2015-42-52.png)

> _**PVST**_: Per VLAN Spanning Tree
> _**PVST+**_: PVST + 802.1q support

- Con PVST podemos definir las topologias de acuerdo a la VLAN en la que se esta trabajando
	- Cada topologia en una VLAN puede ser totalmente independiente de otra 
- PVST viene por defecto en los switches Cisco

El estandar IEEE 802.1d/1w define una instancia de [STP](Project/Networking/CCNA-notas/Spanning%20Tree%20Protocol/(LEGACY)%20STP/STP.md)  en todas las [VLAN](../../Virtual%20LAN/VLAN.md).
Habitualmente se conoce con el nombre de MST (Mono Spanning Tree). No confundir con MSTP (Multiple Spanning Tree).

_IEEE_
- 802.1d (STP)
- 802.1w (RTSP)
- 802.1s (MSTP)

_Cisco_
- PVST
- PVST+
- Rapid-PVST+

#### Exercise
_Ver: [STP - PVST, PVST+ and RPVST+ - Exercise](STP%20-%20PVST,%20PVST+%20and%20RPVST+%20-%20Exercise.md)_