> Recordar: En STP, las palabras _bridge_ y _switch_ se pueden tomar como sinonimos.

**Tipos de puentes (bridges):**
- Root bridge
- Non-root bridge

**Tipos de roles de puertos (ports)**
- Root port
- Designated port
- Non-designated port 

**Estado de las interfaces**
- Blocked
- Listening
- Learning
- Forwarding
- Disabled

**Conceptos de STP**
- _Bridge ID (BID):_ prioridad del bridge (`32768`, default) + VLAN ID (`1`, default) y la MAC address
- _Port ID:_ prioridad del port (`128`) y número de la interfaz (ej: `f0/1`)
- _Path cost:_ costo acumulativo de todos los enlaces entre el switch y el root bridge (RB)