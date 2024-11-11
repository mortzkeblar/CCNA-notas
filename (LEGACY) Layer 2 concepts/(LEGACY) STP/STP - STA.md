---
tags:
  - STP
  - CCNA
---

# Spanning Tree Algorithm


> STA es un algoritmo para [STP](Project/Networking/CCNA-notas/(LEGACY)%20Layer%202%20concepts/(LEGACY)%20STP/STP.md) que permite eliminar los caminos cíclicos de la topología lógica. Quedando como respaldo los caminos físicos en caso de que los primeros fallen.
> Recordar: cuando se habla de _segmento_ en L2, se hace referencia al enlace cableado entre switches.

![](../../_anexos_/Screenshot%20from%202024-01-02%2011-31-24.png)

1. Elección del root bridge (_Ver: [STP - STA, Root bridge election - Root Bridge](STP%20-%20STA,%20Root%20bridge%20election%20-%20Root%20Bridge.md)_)
2. Se determina el root port (RP) en cada switch de tipo non-root (_Ver: [STP - STA, Port roles election - Root port](STP%20-%20STA,%20Port%20roles%20election%20-%20Root%20port.md)_)
3. Se determinan los puertos designados (DP) en cada segmento (_Ver: [STP - STA, Port roles election - Designed ports](STP%20-%20STA,%20Port%20roles%20election%20-%20Designed%20ports.md)_)

Todos los switches envían BDPUs cada 2 segundos (Hello Time) a la _Multicast MAC address_ `01-80-C2-00-00-00`
