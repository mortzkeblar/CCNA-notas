---
tags:
  - ACL
  - concept
---

# Access Control List

Es utilizado para _SELECCIONAR_ flujos de tráfico segun algun criterio especifico. _EJ:_
- IP de origen/destino
- TCP port
- IP traffic type
- etc

A las cuales se les aplica un acción, como pueden ser permitir o bloquear, VPN forwarding, seleccionar los hosts que usarán NAT, etc.

*TIPOS DE ACL*
- `ACL Standard`: seleccionan trafico mirando la dirección de origen del IP header
- `ACL  Extended`: estos usan las direcciones IP de origen y destino, asi mismo los puertos de origen y destino (TCP/UDP en L4).
- `ACL named/numeric`: estas pueden ser numeradas o nombradas.
- `Dynamic ACL (look-and-key)`: para permitir el trafico se utiliza una sesión `telnet`.
- `Reflexive ACL`: Permiten el trafico de vuelta de un router. Muy basicas (mejor CBAC).
- `Time-Based ACL`: permite usar un ACL segun un horario de tiempo especifico.
- `Context-Based ACL (CBAC)`: convierte al router en un Stateful Firewall (como un ASA). Guarda los estados de las conexiones (ACK/SEQs, etc). para permitir el trafico de retorno. Más avanzado que las `Reflexive ACL`.

_Dispositivos orientados a ACL_
- Integrated Service Router (ISR) G2

![](_anexos_/Screenshot%20from%202023-12-28%2009-57-15.png)

Para crear una ACL:
- Definir una lista interesante
- Crear una política de seguridad mediante el comando `access-list` o `ip access-list`
- Aplicar  esa política en una interfaz del router (física o subinterfaz) en modo entrada (in) o salida (out)

