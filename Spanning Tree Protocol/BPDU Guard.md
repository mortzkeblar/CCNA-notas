---
tags:
  - CCNA
  - SpanningTreeProtocol
---
_BPDU Guard_ es una caracteristica [[Project/Networking/CCNA-notas/Spanning Tree Protocol/STP|STP]] opcional que sirva para deshabilitar un puerto si esta detecta que recibe BPDUs. 

Esto debe estar habilitado en los puertos [[PortFast]], ya que si de alguna manera un puerto portfast destinado a host finales es conectado hacia otro switch. BPDU Guard deshabilita el puerto para evitar que se pueda conectar al otro switch, evitando asi que cambie la topologia [[Project/Networking/CCNA-notas/Spanning Tree Protocol/STP|STP]] o que puedan producirse _layer 2 loops_. 

Para habilitar BPDU Guard se puede usar el comando `spanning-tree bpduguard enable`. Tambien se puede usar `spanning-tree portfast bpduguard default` para habilitar BPDU Guard en todos los puertos que tengan habilitado [[PortFast]]. 

![[Pasted image 20241114031517.png]]
