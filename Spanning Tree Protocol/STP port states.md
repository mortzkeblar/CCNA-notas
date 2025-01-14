---
tags:
  - CCNA
  - SpanningTreeProtocol
date created: Thursday, November 14th 2024, 3:05:32 am
date modified: Wednesday, November 20th 2024, 11:33:50 pm
---
Los principales port states en [[Project/Networking/CCNA-notas/Spanning Tree Protocol/STP|STP]] son: _blocking_, _listening_, _learning_ y _forwarding_.

![[Pasted image 20241114014346.png]]

- **Blocking state** - el puerto es desactivado y no puede enviar ni recibir frames, solo escucha los BPDUs para cuando se realizan cambios en la red 
	- Los puertos non-designated deberian tener este estado 
	- Aun aunque el puerto tenga blocking state, en la información de interfaces tiene UP/UP status. Al ejecutar `show spanning-tree` se muestra como `BLK`
- **Listening state** - un puerto llega a este estado cuando es habilitado por primera vez, en este estado los puertos solo pueden enviar y recibir STP BPDUs. No pueden realizar forwarding de frames, y el switch no aprende las MAC address de los frames que puedan pasar por ese puerto.
	- Este estado se utiliza para definir el rol de puerto, ya sea root, designated o non-designated port. 
		- Si el puerto es seleccionado como *non-designated* port, automaticamente pasa al *blocking state*. 
		- Si el puerto es seleccionado como *root* o *designated* port, luego de 15 secs pasan a _learning state_
	- Con valores por defecto, el puerto puede permanecer en este estado transicional por un maximo de 15 segs. 
- **Learning state** - este estado es similar al listening state, con la diferencia de que el switch si comienza a aprender las MAC adddress de los frames que llegan al puerto. 
	- El proposito de este estado es preparar al puerto para realizar el forwarding
	- Es un estado transicional 
		- Luego de pasar 15 segs por este estado, el _root_ o _designated_ port pasa finalmente al _forwarding state_.
- **Forwarding state** - el puerto esta activo y puede reenviar y recibir frames 
	- En un topologia STP, los root port y designated ports tienen el estado forwarding 
Un puerto en learning state aun continua escuchando BPDUs, si ocurre un cambio en la topologia que hace que se vuelva un _non-designated port_, este se va al _blocking state_ inmediatamente.

![[Pasted image 20241114021502.png]]

> Una vez que todos los switches en la red seleccionaron tanto los _port roles_ y todos los puertos se encuentran en estado _forwarding_ o _blocking_, se dice que STP ha **_converged_**. Si se producen cambios en la red, se realiza el calculo de la nueva topologia para luego reconverger nuevamente. 
