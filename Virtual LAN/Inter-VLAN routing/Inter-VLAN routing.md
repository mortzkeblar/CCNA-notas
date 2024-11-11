---
tags:
  - VLAN
---
Este concepto hace referencia al enrutamiento entre subredes en una LAN que esta segmentada mediante [[VLAN]]s. 

El metedo más simple (no recomendado) es el uso de un enlace por cada [[VLAN]] creada, esto tiene el problema de que por la cantidad de VLANs que podemos crear es proporcional a la cantidad de puertos disponibles.

![[Pasted image 20241109192810.png|500]]

Esto es una limitación si la cantidad de VLANs que necesitamos es mayor a la de puertos disponibles. Por lo cual se usan los siguientes metodos:
- [[Router on a stick]] 
- [[Multilayer switching]] 



