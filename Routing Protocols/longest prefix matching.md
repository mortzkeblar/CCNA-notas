---
tags:
  - routing
  - concept
---
Los routers usan la regla de 'longest prefix match' para determinar que rutas se deberían usar para enviar un paquete hacia su destino. 
Esta indica que un router siempre va a preferir la entrada de la tabla de routing más larga o especifica a las entradas menos especificas (p. ej. summary addresses), al determinar que entrada usar para reenviar el trafico.

| Routing Table Entry | Order Used |
| ---- | ---- |
| 1.1.1.1/32 | 1st |
| 1.1.1.0/24 | 2nd |
| 1.1.0.0/16 | 3rd |
| 1.0.0.0/8 | 4th |
| 0.0.0.0/0 | 5th |
## connected
- [building the IP routing table](building%20the%20IP%20routing%20table.md) 
- [example work routing table entry](example%20work%20routing%20table%20entry.md) 


