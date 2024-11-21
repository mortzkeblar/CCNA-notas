---
tags:
  - CCNA
date created: Monday, October 21st 2024, 12:04:56 pm
date modified: Wednesday, November 20th 2024, 11:33:50 pm
---
**The network layer**, es la capa responsable de enrutar los paquetes a su destino de forma end-to-end (La dirección de destino es la misma durante todo el recorrido, a diferencia de L2 donde la MAC address de destino cambia en cada salto). Tambien se encarga de la calidad de servicio (QoS).

Estas dos capas trabajan juntas para permitir que el mensaje llegue a su destino final:
- L2 usa MAC addresses para proporcionar envio hop-to-hop
- L3 usa IP address para proporcionar envio end-to-end

![[Pasted image 20241021095802.png]]

IP (Internel Protoool) es el protocol L3 más usado.
- Tambien tenemos otros protocolos como ICMP y IPSec 

Los protocolos de L3 son no orientadas a conexión, por lo cual no tenemos acknowledgements.

