---
tags:
  - CCNA
---
Router ID es una caracteristica que emplea [[EIGRP]] para la prevención de routing loops. 

> Esta se diferencia de OSPF Router ID, que la emplea para la identificación de vecinos 

En EIGRP, Router ID se usa para identificar las fuentes de las rutas externas. Rutas externas con el mismo RID que el router local son descartadas, esto previene los routing loops cuando rutas externas son inyectadas dentro de un EIGRP [[AS]]. 

El proceso de elección del RID es similar a OSPF, se elije la [[Project/Networking/CCNA-notas/labs/NetWarriors/IP]] más alta configurada en el router. En caso de que hayan loopback interfaces, se prefieren estas por encima de las interfaces fisicas. 

RID puede ser configurado manualmente con `eigrp router-id`, si ya hay configurado una instancia EIGRP, esta se actualiza automaticamente con el nuevo RID y se forma una nueva adyacencia. El RID se puede consultar en la EIGRP topology table. 

```
R1#show ip eigrp topology
IP-EIGRP Topology Table for AS(150)/ID(10.3.3.1)
Codes: P - Passive, A - Active, U - Update, Q - Query, R - Reply,
r - reply Status, s - sia Status
[output truncated]
A RID of 1.1.1.1 is now configured on the router as follows:
R1(config)#router eigrp 150
R1(config-router)#eigrp router-id 1.1.1.1
*Mar 1 05:50:13.642: %DUAL-5-NBRCHANGE: IP-EIGRP(0) 150: Neighbor
150.1.1.2 (Serial0/0) is down: route configuration changed
*Mar 1 05:50:16.014: %DUAL-5-NBRCHANGE: IP-EIGRP(0) 150: Neighbor
150.1.1.2 (Serial0/0) is up: new adjacency
```