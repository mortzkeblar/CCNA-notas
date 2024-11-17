---
tags:
  - STP
  - CCNA
---

> Recordar: En [STP](Project/Networking/CCNA-notas/Spanning%20Tree%20Protocol/(LEGACY)%20STP/STP.md), las palabras _bridge_ y _switch_ se pueden tomar como sinonimos.


### Valores STP
- **Bridge ID (BID)**: prioridad del bridge (`32768`, default) + VLAN ID (`1`, default) y la MAC address
- **Port ID**: prioridad del port (`128`) y número de la interfaz (ej: `f0/1`)
- **Path cost**: costo acumulativo de todos los enlaces entre el switch y el root bridge (RB)

**Siempre se cumple que:**
- Si en un lado existe un _Root Port_, en el otro extremo debe haber un _Designated Port_
- Si en un lado existe un _Non-Designated Port_, en el otro extremo existe un _Designated Port_
### Bridges 
#### Tipos de puentes (bridges)
- Root bridge
- Non-root bridge

#### Tipos de roles de puertos (ports)
- Root port
- Designated port
- Non-designated port 

#### Estado de las interfaces
- Blocked
- Listening
- Learning
- Forwarding
- Disabled


