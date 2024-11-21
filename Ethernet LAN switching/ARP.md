---
tags:
  - CCNA
date created: Wednesday, October 23rd 2024, 3:31:21 am
date modified: Wednesday, November 20th 2024, 11:33:50 pm
---
**Address Resolution Protocol**, este protocolo es el que permite a un host conocer la MAC address de otro host dentro de la LAN. ARP genera dos mensajes:
- ARP Request, pregunta al otro host cual es su MAC address 
- ARP Reply, es la respuesta con la MAC address que llega al host solicitante 

> El mensaje ARP Request es de tipo broadcast, mientras que el ARP Reply de tipo unicast 

El host solicitante para apuntar a su objetivo especifica la dirección IP del cual desea conocer su MAC address. Una vez obtiene la respuesta la agrega a su propia tabla ARP.

![[Pasted image 20241023042301.png|500]]
> En el proceso ARP, los [[switch]]es tambien aprenden las MAC address tanto del solicitante como del receptor


ARP se considera como un puente entre las capas L2/L3 ya que permite asociar direcciones de L3 conocidas con direcciones L2 desconocidas. 