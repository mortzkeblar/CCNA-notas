---
tags:
  - VLAN
  - CCNA
---
La [[VLAN]] nativa es la VLAN a la que esta asignada el trafico _untagged_ que recibe un trunk port ([[IEEE 802.1Q]]). El trafico que esta en la VLAN nativa y pasa por un trunk port, se reenvia sin etiquetar. 

Por defecto, la VLAN nativa es la VLAN 1. Esto se puede comprobar para cada trunk port con `show interfaces trunk`.

> Se debe diferenciar _VLAN native_ y _Default VLAN_.
> - Default VLAN es la VLAN a la que los access ports estan asignados por defecto (no se puede cambiar)
> - VLAN native es la VLAN a la que se asignan los _untagged frames_ cuando son recibidos por un trunk port. Por defecto es la VLAN 1, esta puede ser cambiada por puerto.

Para configurar la VLAN nativa se puede usar `switchport trunk native vlan <vlan-id>`. 

![[Pasted image 20241109184045.png|500]]

## Native VLAN mismatch 
Es importante que al configurar un VLAN Native, deban coincidir en ambos extremos del enlace. Ya que si no se da un problema llamado _VLAN mismatch_. 

![[Pasted image 20241109184208.png|500]]

En este caso el trafico untagged que sale desde SW1 y es asignado a la VLAN 10, pero al llegar a SW2 este lo asigna a la VLAN 30. Haciendo que el frame no 
puede llegar a su destino original.

### Disabling Native VLAN 
Por seguridad es recomendable asignar una VLAN sin uso como VLAN native, esta tiene que diferente de la VLAN 1.

> El cambio de la Native VLAN puede acarrear problemas si se usa dispositivos que no soportan [[IEEE 802.1Q]] como los hubs. 




## Security risks 
- Problemas de seguridad asociada a la VLAN Nativa (1)
	- https://learningnetwork.cisco.com/s/blogs/a0D3i000002SKPREA4/vlan1-and-vlan-hopping-attack
	- https://networkdirection.net/articles/network-theory/taggeduntaggedandnativevlans/