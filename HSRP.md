---
tags:
  - CCNA
date created: Saturday, October 19th 2024, 11:08:14 pm
date modified: Wednesday, November 20th 2024, 11:33:50 pm
---
- Hot Stanby Redundancy Protocol
- Es un protocolo propietario de Cisco (VRRP es estander de la IETF)
- Multicast (224.0.0.2 en HRSPv1 y 224.0.0.102 en HSRPv2) - UDP 1985
- No soporta balanceo de carga (pero se puede realizar un pseudo balanceo con VLANs)
- Active/Standby elegidos por prioridad mayor (Por defecto: 100. Rango 0-255)
- Envia "Hellos" cada 3 segundos
- Soporta "tracking" de objetos 


``` bash
int g0/0
ip address 192.168.0.1 255.255.255.0
standby 1 version 2
stanby 1 ip 192.168.0.1 # ip de gateway en los hosts
stanby 1 priority 150
stanby 1 preempt

stanby 1 track 60
```