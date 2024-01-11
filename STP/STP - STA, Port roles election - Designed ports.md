En este punto debemos calcular cuales interfaces quedan en rol _**Designated**_ y cuales quedan en _**Non-designated**_.

![](../_anexos_/Screenshot%20from%202024-01-02%2012-34-47.png)

El _root bridge_ genera las BPDUs con costo inicial 0. Cada switch toma la BPDU recibida en su _root port_ yu la reenvía por las demás interfaces añadiendo el costo contenido dentro de la BPDU al costo del puerto donde la recibió.
- En el ejemplo, S2 reenvía el BPDU de S1 a S3 con el costo que recibio (es decir, 19)
	- Al llegar a S3, este le suma otro costo de 19, quedando en 38.
- Lo mismo ocurre a la inversa, como ambos tiene los mismos valores. No es suficiente para poder elegir los puertos designados.

En caso de empate, STP contempla los siguientes casos hasta encontrar uno que pueda generar el desempate:
1. Prefiere siempre el switch con el Root Bridge ID más bajo
2. Prefiere siempre el switch que tenga el costo de ruta más baja hacia el Root Bridge
3. Prefiere siempre el switch que emita la BPDU con Bridge ID más bajo
4. Prefiere siempre el switch que emita la BPDU con el Port ID más bajo

En cada segmento donde no hay un RP (_root port_), STP seleccionará un extremo como DP (_designated port_) en esta de reenvío (_Forwarding_) y otro como _Non-DP_ en estado _Blocking_.

_En nuestro ejemplo, el desempate surge con la opción nro. 3, esto implica que la ruta entre S2 y S3 queda eliminada (a nivel lógico) pero S2 queda en modo Forwarding a la espera de otra conexión, S3 queda bloquado._

![](../_anexos_/Screenshot%20from%202024-01-02%2012-46-27.png)
