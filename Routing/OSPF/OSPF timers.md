---
tags:
  - routing
  - dynamic
  - OSPF
  - CCNA
date created: Saturday, October 19th 2024, 11:08:14 pm
date modified: Wednesday, November 20th 2024, 11:33:50 pm
---

[OSPF](OSPF.md) usa varios timers (temporizadores) para el control broadcast, link state propagation y otras operaciones. 

Los timers por defecto pueden variar dependiendo el [[OSPF network types]] que se este configurando. 

> Es importante que los timers deban coincidir entre sí para que se forman que se puedan formar relaciones [[OSPF neighbors and adjacencies]]. 

- _Hello interval_ - el valor por defecto de este timer depende enteramente en que tipo de interface [OSPF](OSPF.md) esta operando. 
	- Para interfaces broadcast (como ethernet) el valor por defecto es 10 segs.
	- Para interfaces Non-broadcast multi-access (como Frame relay) el valor es 30 segs. 
	- Para cambiar esos valores manualmente ejecutar `ip ospf Hello-interval 40`, al hacer esto, Dead y Wait también cambiaran sus valores. 
- _Dead interval_ - se define como el tiempo que se tarda en declarar Dead a un vecino si no hay ningun _Hello_. El valor de este timer es 4 veces el de _Hello interval_. Para modificar su valor por defecto: `ip ospf dead-interval 240`. 
	- Si una de las interfaces por algún fallo físico hace que quede en estado DOWN/DOWN, el vecino es removido inmediatamente sin esperar el _dead interval_. 
- _Retransmit interval_ - este valor cambia el intervalo de retransmición entre vecinos. Cuando [OSPF](OSPF.md) envia un update a el router vecino, espera recibir un _acknowledgment (acuso de recibo)_. Si el acknowledgment no llega, se produce una retransmición. El valor por defecto es de 5 segs, este se puede modificar con `ip ospf retransmit-interval 10`.
- _Wait interval_ - es la intervalo que rompe el periodo de espera y causa la designación del router para la red. Este valor siempre es igual al del _Dead timer_. 