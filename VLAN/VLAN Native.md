---
tags:
  - VLAN
  
---
Esta [VLAN](VLAN.md) tiene la particularidad de ser la unica VLAN en el que los frames hacia/desde la VLAN nativa, se transportan sin etiquetar dentro de la red (untagged). Es decir, todos los paquetes que no tiene como destino una VLAN en particular, se transportan a través de la VLAN nativa. 


En Cisco (y otros vendors), tambien esta es la _VLAN default_. Esta ya viene creada y es la única VLAN que no se puede eliminar.  

## Security risks 
- Problemas de seguridad asociada a la VLAN Nativa (1)
	- https://learningnetwork.cisco.com/s/blogs/a0D3i000002SKPREA4/vlan1-and-vlan-hopping-attack
	- https://networkdirection.net/articles/network-theory/taggeduntaggedandnativevlans/