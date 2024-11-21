---
tags:
  - routing
  - dynamic
  - OSPF
  - CCNA
date created: Saturday, October 19th 2024, 11:08:14 pm
date modified: Wednesday, November 20th 2024, 11:33:50 pm
---

En [OSPF](OSPF.md), adyancencia se refiere a la relación establecida entre dos routers vecinos conectados directamente. Esta relación permite el intercambio de enrutamiento como updates de LSAs para calcular las mejores rutas en la red. 

La formula para calcular las adyancencias en una red es $n(n-1)/2$ (siendo $n$ la cantidad de routers en la red). 

## establishing adjacencies
En general antes de que dos routers lleguen a un estado de adyancencia, suelen pasar por varios estados. Donde intercambian información de estado y verifican que se cumplan ciertos criterios para avanzar al siguiente estado.

- **Down** - es el primer estado OSPF, todavia no se recibieron paquetes Hello de un vecino en esa interface.
- **Attempt** - este solo es valido en redes non-broadcast. En este estado, los paquetes Hello son enviados pero no se recibio información de la configuración del vecino.
- **Init** - este estado se logra cuando se reciben los paquetes Hello de un vecino. Para avanzar al siguiente estado deben cumplirse algunos parametros como:
	- OSPF area 
	- Timer values
	- Authentication match
- **2-way** - esto indica que la comunicación bi-direccional entre dos vecinos se ha establecido. Un router pasa a este estado cuando ha recibido un paquete Hello con su propio [RID](RID.md) en el campo _neighbor_ y los parametros del paquete Hello hacen match en los dos routers. En redes multi-access, los routers [DR and BDR](OSPF/DR%20and%20BDR.md) son elegidos en este paso. Los vecinos [OSPF](OSPF.md) que no son DR/BDR (dos DRothers) no pasan de este estado. 
- **Exstart** - aqui se inicializa la sincronización de la database, los vecinos eligen un _master_ y un _slave_. Los routers OSPF intercambian paquetes _Database descriptor (DBD)_. Estos contienen solo los headers de los LSA, describen el contenido de todo el _link state database_. 
- **Exchange** - este estado es donde los routers describen sus _Link State Databases_ usando paquetes DBD. Cada paquete DBD debe ser reconocido explicitamente y el router emisor solo permite un DBD a la vez. Los routers además usan paquetes _Link State Request (LSR)_ para solicitar una instancia de el LSA. Cuando se solicita información faltante, se activa el bit M (More) indicando que falta información, cuando el intercambio de la DB se completa el bit M se pone en 0. 
- **Loading** - los routers OSPF envian paquetes LSR para solicitar instancias LSA más recientes que se recibieron en el estado de **Exchange**. Las actualizaciones enviadas son colocadas en un _link state retransmission_ hasta que la confirmación es recibida. Cuando los routers OSPF reciben LSRs, este responde con LSU (link state update) el cual contiene la información requerida. 
- **Full** - este estado final muestra que las DB estan hicieron el intercambio completamente y ambos vecinos tiene una visión de la topologia de la red identica. Las relaciones se añaden a la DB local y se anuncia en un paquete LSU, además se calculan lás mejores rutas usando _Dijkstra's algorithm_ para añadir a la [Routing Table](Routing%20Table.md). 

![](16-4-scaled.jpg)

Para poder depurar el proceso de adyancencia puede usar `debug ip ospf adj`. Tenga cuidado al usar porque puede enviar una gran cantidad de información que puede saturar el router. 