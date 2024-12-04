---
tags:
  - routing
  - dynamic
  - OSPF
  - CCNA
date created: Saturday, October 19th 2024, 11:08:14 pm
date modified: Wednesday, November 20th 2024, 11:33:50 pm
---

_Network Type_ es una configuración en una interface [[OSPF]] que sirve para determinar como opera OSPF en una red particular. Esto influye en aspectos como los [[OSPF timers]], elección DR/BDR y si todas las _neighbor relationship_ se convertiran en _full adjacencies_.


**OSPF network types (covered in CCNA)**
![[Pasted image 20241203053611.png]]


- **Point-to-point** - un ejemplo de esto es un enlace simple T1 entre dos routers o una conexión frame relay point-to-point. 
	- _Hellos_ son multicast a `224.0.0.5` (AllSPFRouters)
	- No hay elección DR/DBR en este tipo de red.
- **Broadcast** - p. ej. ethernet, para más precisión nos referimos a este tipo como _Broadcast multi-access network_ porque se puede conectar varios dispositivos para que puedan recibir el mismo paquete. 
	- En este tipo de red se elije un DR/BDR.
	- Los _Hellos_ son multicast en `224.0.0.5`, al igual que todos los paquetes [OSPF](OSPF.md)  originados por el DR/BDR. 
	- Los demás routers hacen actualizaciones multicast y paquetes acknowledgment en `224.0.0.5`, tambien conocidos como _ALLDRouters_. 
- **NBMA** - incluye frame relay o conecciones multipoint.
	- los paquetes multicast no son reenviados a los vecinos porque no hay capacidad de broadcast. 
	- Los vecinos [OSPF](OSPF.md) deben ser configurados por el administrador de red a través del comando `mode on DR/BDR`. 
	- El DR/BDR elegido debe ser un router hub (un router con un circuito con todos lo demás routers). 
- **Point-to-multipoint** - este tipo de red debe ser definido en una red NBMA por el administrador de red.
	- No hay DR/BDR.
	- Los paquetes [OSPF](OSPF.md) son multicast.
	- Se necesita mapear la [IP address](IP%20address.md) remota a la dirección L2 (DLCI para Frame Relay) y añadir la palabra broadcast para que [OSPF](OSPF.md) puede hacer multicast de sus paquetes Hello. 
	``` bash
		interface Serial0/1
		ip address 10.0.0.1 255.255.0.
		encapsulation frame-relay
		ip ospf network point-to-multipoint
		frame-relay map ip 10.0.0.2 20 broadcast
		no frame-relay inverse-arp
	```
- **Virtual links** - estos son usados para enlazar areas que no estan conectadas directamente al Area 0. _Ver: [OSPF virtual links](OSPF%20virtual%20links.md)_.


![400](15-5.jpg)


## Broadcast network type 
_Broadcast_ es el network type por defecto en cualquier interface ethernet. Esto se puede comprobar con `show ip ospf interface <interface>`.

La principal caracteristica es la elección de **Designeted Router (DR)** y un **Backup Designeted Router** en el estado _2-way de [[OSPF neighbor states]]_.
- Los routers del mismo segmento que no son DR ni BDR se convierten en **DROthers**
- Los router _DR_ y _DBR_ establecen adyacencias con todos los routers en el segmento. 
- Los routers _DROther_ solo establecen _full adjacencies_ con el _DR_ y _DBR_.
	- Con los otros _DROthers_ permanecen como vecinos en el estado _2-way_, no intercambiar [[LSA]]s entre sí.

El proposito de los DR/DBR es para reducir la cantidad de trafico generado por el intercambio de [[LSA]]s (que se envían mediante LSUs) en un segmento. 

![[Pasted image 20241203062953.png]]

> Los _DROthers_ para enviar mensajes solo al DR/DBR utilizan la IP multicast `224.0.0.6`.
> - `224.0.0.5`, todos los routers 
> - `224.0.0.6`, solo routers DR y DBR


Algo a destacar es que la elección de DR/DBR se hace por _segmentos_, no por _areas_. Definimos segmento como una red compartida donde dos o más routers se pueden comunicar de forma directa. 

![[Pasted image 20241203070535.png]]

En la imagen, se puede ver que las interfaces G0/1 en su segmente estan como DR a pesar de no haber adyacencias. Estas interfaces deben configurarse como [[passive interface]] para que no puedan enviar OSPF hello mensajes. 

Para ver el estado de adyacencia con los otros vecinos se puede usar `show ip ospf neighbor`.

![[Pasted image 20241203071236.png]]

### DR / DBR election 
La elección del DR / DBR se basa en los siguientes criterios:
- _Priority_ más alta de una interface - (por defecto con valor $1$) el valor de prioridad más alta y el segundo más alto son elegidos como DR y DBR respectivamente
	- Se puede establecer el valor de la prioridad con `ip ospf priority <value>` en el modo interface config
- _[[OSPF#Router ID]]_ más alta - el RID se usa como desempate cuando los valores de prioridad son iguales

Para que los cambios hagan efecto, es necesario ejecutar `clear ip ospf process`. El problema con esto es que si ya existe un DR / DBR, el DBR va a tomar la posición de DR.
- Si queremos que un router especifico sea DR, es necesario ejecutar `clear ip ospf process` y verificar que sea elegido como DR. Esto va a pasar una vez que nuestro router sea elegido BDR y ejecutemos el comando una vez más. 

### Point-to-Point network type  
Una conexión _point-to-point_ es un enlace directo entre dos routers. Cuando se usa este tipo de red, se elimina el proceso de elección DR/DBR ya que de todas formas ambos routers forman _full adjacencies_ por lo que es innecesario, en este caso al eliminar el proceso de elección la adyacencia toma menos tiempo.

![[Pasted image 20241203235222.png]]

Para configurar un point-to-point network type se hace uso del comado `ip ospf network point-to-point` en el modo interface config.

![[Pasted image 20241203235422.png]]








> Es importante considerar que a diferencia de una red EIGRP (p. ej.), [OSPF](OSPF.md) no es tolerante a topologias de red arbitrarias, esto implica que se debe ser muy cuidadoso al planear la implementación de una red OSPF, esto junto su esquema de [IP address](IP%20address.md) jerárquicos. 
> 
> La topologia de red y el route summarization, la adopción de un entorno de direccionamiento jerarquica y una asignación de direcciones estructurada son lo más importante a la hora de determinar la escalabilidad de su internetwork (Cisco Press). 

