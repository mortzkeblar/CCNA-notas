---
tags:
  - CCNA
---
**Virtual Router Redundancy Protocol (VRRP)** es una standard [[FHRP]] definido por el IETF e respuesta a [[HSRP]]. 
- VRRP usa la términos _master_ / _backup_ para llamar a los routers que participan en el proceso VRRP 
- _Preemption_  (definición en [[HSRP]]) - se encuentra habilitado por defecto 
- VRRP envia mensajes multicast a la IP 224.0.0.18, una dirección reservada solo para el uso de VRRP 
- La Virtual MAC address tiene el formato `0000.5e00.01XX`, siendo `XX` el número del grupo [[VRRP]]
- Se puede realizar load balancing del ya que permite configurar los _master_ routers de forma independiente por cada subred 