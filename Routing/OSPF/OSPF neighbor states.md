---
tags:
  - CCNA
---

![[Pasted image 20250105033950.png]]

En general antes de que dos routers lleguen a un estado de adyancencia, suelen pasar por varios estados. Donde intercambian información de estado y verifican que se cumplan ciertos criterios para avanzar al siguiente estado.

Para verificar el estado de router OSPF con sus vecinos se usa el comando `show ip ospf neighbor`.

![[Pasted image 20241201231450.png]]

- **Down** - es el primer estado OSPF cuando se configura por primera vez, todavia no se recibieron paquetes Hello de un vecino en esa interface.
	- En este estado solo se envian mensajes multicast pero aún no se conoce a los vecinos, por lo cual el _neighbors table_ se encuentra vacia.
		- Por este motivo, el estado **DOWN** no se suele considerar técnicamente un estado. Ya que el estado esta asociado al router ID del neighbor y el estado que se mantiene, en este caso, la tabla de vecinos esta vacia por lo que no hay ningún estado al que hacer referencia. 
- **Attempt** - este solo es valido en redes non-broadcast. En este estado, los paquetes Hello son enviados pero no se recibio información de la configuración del vecino.
- **Init** - un router ingresa en este estado cuando recibe paquetes Hellos de un vecino. Para avanzar al siguiente estado deben cumplirse algunos parametros como:
	- OSPF area 
	- Timer values
	- Authentication match
- **2-way** - esto indica que la comunicación bi-direccional entre dos vecinos se ha establecido. Un router pasa a este estado cuando ha recibido un paquete Hello con su propio [[OSPF#Router ID]] en el campo _neighbor_ y los parametros del paquete Hello hacen match en los dos routers. 
	- Los vecinos [OSPF](OSPF.md) que no son DR/BDR (un enlace que conecta DRothers) se mantienen en este estado. 
	- En redes multi-access, los routers [(LEGACY) DR and BDR]((LEGACY)%20DR%20and%20BDR.md) son elegidos en este paso. 
- **Exstart** -  en este proceso y en **Exchange** es donde los routers OSPF intercambian mensajes _Database description (DBD)_. Estos contienen solo los headers de los LSA, describen de forma resumida el contenido del [[OSPF#Link-State Database]] de cada router. 
	- En este estado se inicializa la sincronización de la database, los vecinos eligen un _master_ y un _slave_, en base al quien tenga el [[OSPF#Router ID]] más alto. 
- **Exchange** - este estado es donde los routers describen sus [[OSPF#Link-State Database]] (LSDB) de forma resumida usando paquetes DBD. Cada paquete DBD debe ser reconocido explicitamente y el router emisor solo permite un DBD a la vez. 
	- En este proceso es donde se identifican discrepancias en sus LSDBs, marcando la información faltante para completar su database.  
		- El bit `M` (more) indica si hay paquetes DBD faltantes, cuando no queda información adicional para compartir, este bit se pone en `0`.   
- **Loading** - en este punto cada router ya conoce la [[OSPF#Link-State Database]] de sus vecinos. 
	- En **loading** state, los routers envían mensajes _Link State Requests (LSR)_ para solicitar [[LSA]]s especificas entre sí. Estas [[LSA]]s son enviadas mediante _Link-State Update (LSU)_, que a su recepción son confirmadas con mensajes _Link-State Acknowledgment (LSAck)_.
		- [[LSA]]s son data stractures que contiene información de enrutamiento OSPF, estas son enviadas en  mensajes _LSU_ que actuan como carriers. 
- **Full** - estado final donde indica que las LSDBs de los routers estan sincronizadas y los vecinos tiene una visión de la topologia de la red identica. Los routers son llamados _adjacent neighbors_ o _full adjacent neighbors_. 
	- Mientras las relaciones se mantengan, estas van compartiendo [[LSA]]s a medida que cambia la red. 

> Para poder depurar el proceso de adyancencia puede usar `debug ip ospf adj`. Tenga cuidado al usar porque puede enviar una gran cantidad de información que puede saturar el router. 

![[Pasted image 20250105034030.png|500]]
![[Pasted image 20250105034112.png|500]]
![[Pasted image 20250105034129.png|500]]



Incluso luego de que los _neighbor relationships_ se hayan establecido, estos continuan mandando mensajes _Hello_ en intervalos regulares para asegurar que la relación continue vigente, los tiempos y valores definidos se ven en [[OSPF timers]]. 