---
tags:
  - ETHERCHANNEL
  - CCNA
---

Es una tecnologia desarrollada por Cisco destinada a funcionar entre switches, la idea detras de _Etherchannel_ es ofrecer mayor ancho de banda disponible para los enlaces que tienen una carga superior a la media.
- Funciona por ejemplo al tener enlaces redundantes pero en segmentos que son paralelos
	- Aunque sean paralelos forman un bucle de conmutación , entonces [STP](../STP/STP.md) actua para eliminar uno de los enlaces

![](_anexos_/Screenshot%20from%202024-01-04%2016-47-27.png)

Si lo que necesitamos es usar todos los enlaces simultáneamente. Necesitamos habilitar _Etherchannel_, el cual convierte todos los enlaces físicos en un solo enlace lógico.

> No confundir la funcionalidad del  _etherchannel_ con la suma del ancho de banda de los enlaces fisicos. 
> _Ej:_ si hacemos etherchanel sobre dos enlaces de 1Gb cada una, la fusión de ambas sigue dando como resultado 1Gb. Solo que permite usar ese ancho de banda de forma paralela.
> 

Considerar que si usamos dos enlaces paralelos que usan _etherchannel_, STP va a actuar de igual manera porque va a detectar un bucle (aun asi los dos enlaces sean etherchannel).

Los situaciones donde se suele usar los _etherchannel_ son
- Multiples conexiones en switches
- Servidor de virtualización


## Tipos de etherchannel 
- _[Manual](Etherchannel%20-%20Manual.md),_ ambos lados deben ser configurados manualmente en esta modalidad
- _Dynamic_, en este caso etherchannel se encarga de enviar mensajes entre los distintos puntos para poder formar enlaces. Tenemos dos tipos actualmente:
	- _[[LACP (Link Aggregation Control Protocol)]]_, IEEE 802.3ad
	- _[PAgP  (Port Aggregation Protocol)](Etherchannel%20-%20Dynamic,%20PAgP.md)_, Protocolo propietario de Cisco

> Importante: etherchannel aumenta el ancho de banda disponible al balancear la carga de los enlaces. El metodo por defecto de balance de carga es utilizar la MAC address origin.

Las tramas provenientes de diferentes MAC se enviaran por distintos puertos, pero las tramas provenientes de una misma MAC se enviaran por el mismo puerto de salida.
### Algunos detalles
- No podemos asociar dos interfaces diferentes (p. ej. un fast ethernet con un gigabit ethernet)
- Etherchanel proporciona un ancho de banda de 800mb/s full duplex con una conexión fast ethernet 
- Etherchanel proporciona un ancho de banda de 8gb/s full duplex con una conexión gigabit ethernet 
- No puedes enviar trafico a dos dispositivos diferentes a través de un mismo etherchanel 
- La conexión de los puertos que estan asociados a un etherchanel deben ser igual en ambos extremos
### L2 and Trunk
[Etherchannel - L2 and Trunk Mode](Etherchannel%20-%20L2%20and%20Trunk%20Mode.md)
### L3
[Etherchannel - L3](Etherchannel%20-%20L3.md)
### Show summary
[Etherchannel - Show summary](Etherchannel%20-%20Show%20summary.md)

### Cuando se formará un etherchannel?
![](_anexos_/Screenshot%20from%202024-01-04%2017-30-07.png)

### Requerimientos para formar etherchannel
_Ver: [[Etherchannel - Requeriments]]_

### Opciones de balanceo
![](_anexos_/Screenshot%20from%202024-01-04%2017-07-24.png)

### Etherchannel Exercise
_[Etherchannel - Exercises, Topology](Etherchannel%20-%20Exercises,%20Topology.md)_