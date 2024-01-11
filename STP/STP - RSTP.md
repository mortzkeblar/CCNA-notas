# Rapid Spanning Tree Protocol
## SPT vs RSPT - Differences
### Port State - Traditional STP
![](../_anexos_/Screenshot%20from%202024-01-04%2011-33-33.png)
Como se puede ver en el grafico, una de las prinicipales diferencias entre RSTP y STP tradicional (legacy) es que los estados de SPT _disable, blocking y listening_ son fusionados en un solo estado _discarding_, los demas se mantienen.
Esto permite aumentar bastante la velocidad de transición para poder habilitar una interfaz.

> Se considera que tanto _Listening_ como _Learning_ son estados transitorios y que los puntos finales siempre son _Forwarding_ o _Blocking_.

### IEEE - 802.1D vs 802.1W
![](../_anexos_/Screenshot%20from%202024-01-04%2011-42-23.png)
- **Alternate Port**, es un método de respaldo de _Root Port_ de cada switch. Si fallae el Root Port, se utilizara _Alternete_ como Root Port.
- **Backup Port**, es un método de respaldo para un _Designated port_.
- Si un switch ejecutando RSTP recibe un BPDU 802.1D (SPT legacy), solamente ese puerto se comportara como 802.1D por compatibilidad.


## RSTP Port Types
- **Edge Port**. se configuran mediante el comando `switchport portfast` y conectan estaciones finales o servidores. Estos puertos pasan inmediatamente al estado _Forwarding_
- **Non-edge Port**, puede ser de dos tipos
	- _Point-to-Point_, identificados por operar en modalidad _Full-Duplex_ con otros switches
	- _Shared Ports_, puertos conectados a otros switches pero en modalidad _Half-Duplex_. En realidad el proceso de sincronización (_Agreement_ y _Proposal_) no opera en puertos compartidos, por lo tanto no habrá nunca convergencia rapida en estas interfaces.

Configuración en _Global mode_:
``` bash
Switch(config)$ spanning-tree mode rapid-pvst
```

### Example
``` bash

S1(config)$ show spanning-tree
...
	Spanning tree enabled protocol ieee
...
## Este mensaje indica que se usando la version legacy de STp

# Para cambiar a RSPT
S1(config)$ spanning-tree mode rapid-pvst
S1(config)$ show spanning-tree
...
	Spanning tree enabled protocol rstp
...

# Se puede la prioridad segun la VLAN en la que se trabaja 
S1(config)$ spanning-tree vlan 1 priority 4096

# Se recomiendo usar portfast solo en interfaces 'trunking' y en los que conectan a host terminales
S1(config)$ spanning-tree portfast
```