---
tags:
  - DHCP
  - CCNA
---


_Ver: [IP traffic types](IP%20traffic%20types.md) _

> ![](../_anexos_/Screenshot%20from%202024-01-01%2014-05-22.png)
> 
> _Ejemplo_:
> Un host que acaba de conectarse fisicamente a la red. Como no esta configurado de antemano, envia un mensaje _broadcast_ preguntando si existe un servidor DHCP (DHCP discover).
> - Si no hay un servidor DHCP, vuelve a enviar mensajes broadcast. Si en todo ese transcurso no recibe nada entra en modo _APIPA_ (Automatic Private IP Addressing) y se asigna una IP 169.254.X.X (estas direcciones no son enrutables, link-local). 
>   Si otro host se conecta y entra la misma situación, entre en modo _APIPA_ lo que le permite conectarse con el host anterior. 
> - Si hay un servidor DHCP, el servidor responde con un mensaje _OFFER_ (ofrecer). Como esta en la red LAN, este ya ha tomado la MAC address y puede enviar un mensaje _unicast_ directamente sin necesidad de inundar la red. 
>   El host responde al _offer_ con un mensaje _REQUEST_ solicitando información IP al servidor, como el host aun no esta configurado, necesita enviar un mensaje _broadcast_.
>   El servidor puede responder (en forma _unicast_) de dos formas a ese mensaje: ACK o NACK. 
>   - ACK se envia cuando el host envia correctamente los datos de conexión.
>   - NACK puede deberse a muchas razones, el más comun suele ser que ya no hay más IP disponibles. Otra razon puede ser que existe una blacklist.  

_ACK: el concepto de ACK es cuando se envia un dato de origen/destino y ese dato es confirmado por el receptor de que llego correctamente (en español, ACUSO DE RECIBO)._

## DHCP Discover Example - Wireshark
![](../_anexos_/Screenshot%20from%202024-01-01%2014-06-51.png)
- Se puede ver un DHCP discover hecho por _nadie (0.0.0.0)_ y enviado a _todos (255.255.255.255)._
- Se puede ver que la información esta dirigida desde la MAC address L2, pero si vemos en L3 aun no tiene ninguna configuración de IP. 