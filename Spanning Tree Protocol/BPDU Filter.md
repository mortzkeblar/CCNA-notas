---
tags:
  - CCNA
  - SpanningTreeProtocol
---
_BPDU Filter_ es una caracteristica [[Project/Networking/CCNA-notas/Spanning Tree Protocol/STP|STP]] que se puede usar para prevenir que se envien o reciban BPDUs en un puerto especifico. Esto es deseable en puertos donde [[Project/Networking/CCNA-notas/Spanning Tree Protocol/STP|STP]] no es necesario (como en los puertos de hosts finales).

BPDU Filter puede ser habilitado de dos formas diferentes, con diferentes resultados dependiendo la forma que se habilite. 
- Habilitar BPDU Filter en un puerto especifico 
	- Se puede usar `spanning-tree bpdufilter enable` en el modo config interface 
	- Esto puerto no envia BPDU e ignora cualquier BPDU que reciba
	- Esto deshabilita STP en el puerto 
- Habilitar BPDU Filter globalmente para todos los puertos donde [[PortFast]] esta habilitado 
	- Se puede usar `spanning-tree portfast bpdufilter default` en el modo global config 
		- Esta habilita BPDU Filter para todos los puertos [[PortFast]] ([[RSTP]] edge ports)
	- El puerto no envia BPDUs 
	- Si el puerto recibe un BPDU, tanto [[PortFast]] como BPDU Filter se deshabilitan y el puerto pasa a funcionar como un puerto [[Project/Networking/CCNA-notas/Spanning Tree Protocol/STP|STP]]/[[RSTP]] normal 

> BPDU Filter no es recomendable de usar, existe riesgo de que genere un layer 2 loop si se conecta un [[switch]] a un puerto que lo tenga habilitado (esto puede ocurrir cuando se configura en el config mode ya que en este modo [[Project/Networking/CCNA-notas/Spanning Tree Protocol/STP|STP]] se encuentra deshabilitado). 