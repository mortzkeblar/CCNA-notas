Bridge Protocol Data Unit (BPDU) es el mecanismo para que STP pueda funcionar. 
A grandes rasgos consiste en un mensaje que envia a través de la red tratando de descubrir los caminos ciclicos (o _bucles_) que puedan existir en la red, tambien realiza configuraciones/modificaciones de ser necesaria.
- _Solicitud:_ TCN (Topology Change Notification)
- _Respuesta:_ TCA (Topology Change Acknowledgement)

> En palabras simples: se puede decir que funciona como un _boomeran_ que lanza el mensaje a todos sus vecinos (de forma simultanea) esperando que la respuesta llegue por otra interfaz, lo cual le indica que formo un camino ciclico. 

## Composición de la BPDU

![](../_anexos_/Screenshot%20from%202024-01-02%2011-21-26.png)

- Los campos en naranja se fueron agregando a medida que se fue desarrollando STP.

#### BPDU  - Bridge ID Field

![](../_anexos_/Screenshot%20from%202024-01-02%2011-23-14.png)

- `Extended System-ID` es un campo que se agrego en la versión actual de STP (IEEE 802.1D-2004)

![](../_anexos_/Screenshot%20from%202024-01-02%2011-25-25.png)

#### Proceso de convergencia de STA
_Ver: [STP - STA](STP%20-%20STA.md)_