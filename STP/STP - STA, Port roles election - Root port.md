> Para ver los valores de las distintas unidades, _ver: [STP - Bridges](STP%20-%20Bridges.md)_

Luego de elegir al Root bridge, cada switch deberá asignar un rol a sus puertos calculando el costo acumulativo hacia el Root bridge (**STP Root Path Cost**) por cada una de sus interfaces.
La interfaz de menor costo se designa como  Root Port (RP).

- El Root Bridge tiene siempre todos sus puertos en rol _Designated (DSGN)_
- Cada switch Non-root tiene obligatoriamente al menos 1 RP
- Como en este punto ya tenemos un RB, solo este puede enviar BPDUs
	- Es decir, los switches Non-root solo puede reenviar el mensaje BPDU del RB pero ellos no puede generarlo

![](../_anexos_/Screenshot%20from%202024-01-02%2012-17-30.png)

## Valores por defecto de los _Port Costs_, según los estandares STP lanzados

![](../_anexos_/STP_default_port_costs.jpg)
_Para nuestro ejemplo concreto y por simpleza, usamos los valores del estandar STP 1998_.

En caso de que un switch tenga más de un camino y todos tengan el mismo valor de costo, se debe consultar la lista que permite el desempate _Ver: [STP - STA, Port roles election - Designed ports](STP%20-%20STA,%20Port%20roles%20election%20-%20Designed%20ports.md)_