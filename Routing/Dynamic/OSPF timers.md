[OSPF](OSPF.md) usa varios timers (temporizadores) para el control broadcast, link state propagation y otras operaciones. 

Los timers por defecto pueden variar dependiendo el tipo de red en el que se esta configurando [OSPF](OSPF.md) (p. ej. point-to-point o non-broadcast). 

Los timers deben coincidir para que se forman relaciones entre vecinos [OSPF](OSPF.md) del mismo área. En el ejemplo de abajo, Router1 y Router2 no formaran una relación de vecinos porque sus timers son diferentes.

``` bash
Router1#show ip ospf interface Serial0/0
Serial0/0 is down, line protocol is down
Internet Address 192.168.1.1/24, Area 0
Process ID 20, Router ID 192.168.1.2, Network Type POINT_TO_POINT, Cost: 64
Transmit Delay is 1 sec, State DOWN,
Timer intervals configured, Hello 10, Dead 40, Wait 40, Retransmit 5

Router2#show ip ospf interface Serial0/0
Serial0/0 is down, line protocol is down
Internet Address 192.168.1.2/24, Area 0
Process ID 20, Router ID 192.168.1.2, Network Type POINT_TO_POINT, Cost: 64
Transmit Delay is 1 sec, State DOWN,
Timer intervals configured,Hello 30, Dead 120, Wait 120, Retransmit 5

## Solo coincide el timer retransmit con el valor 5
```

- _Hello interval_ - el valor por defecto de este timer depende enteramente en que tipo de interface [OSPF](OSPF.md) esta operando. 
	- Para interfaces broadcast (como ethernet) el valor por defecto es 10 segs.
	- Para interfaces Non-broadcast multi-access (como Frame relay) el valor es 30 segs. 
	- Para cambiar esos valores manualmente ejecutar `ip ospf Hello-interval 40`, al hacer esto, Dead y Wait también cambiaran sus valores. 
- _Dead interval_ - se define como el tiempo que se tarda en declarar Dead a un vecino si no hay ningun _Hello_. El valor de este timer es 4 veces el de _Hello interval_. Para modificar su valor por defecto: `ip ospf dead-interval 240`. 
- _Retransmit interval_ - este valor cambia el intervalo de retransmición entre vecinos. Cuando [OSPF](OSPF.md) envia un update a el router vecino, espera recibir un _acknowledgment (acuso de recibo)_. Si el acknowledgment no llega, se produce una retransmición. El valor por defecto es de 5 segs, este se puede modificar con `ip ospf retransmit-interval 10`.
- _Wait interval_ - es la intervalo que rompe el periodo de espera y causa la designación del router para la red. Este valor siempre es igual al del _Dead timer_. 