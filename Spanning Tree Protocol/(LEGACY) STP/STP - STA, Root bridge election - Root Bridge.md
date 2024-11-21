---
tags:
  - STP
  - CCNA
date created: Saturday, October 19th 2024, 11:08:14 pm
date modified: Wednesday, November 20th 2024, 11:33:50 pm
---

> Referencia: para no mezclar los conceptos acerca de los valores con respecto a la prioridad. Recordar que:
> - En L3, los protocolos que usan prioridad en sus procesos, suelen elegir el valor más alto para tomar una desición
> - En L2 (como vemos en STP), el valor de prioridad que se usa como ser el más bajo



Todos los switches comienzan enviando BPDUs de tipo _Hello_ para intercambiar información respecto a sus BIDs (Bridge ID). 

- Cada BPDU incluye el BID y Root BID como si mismo (en otras palabras, cada switch cree que es el Root bridge)
-  Los campos más importantes en este proceso es _RID_ y _BID_

Cada switch compara las BDPU recibidas contra su propio valor de Root Bridge. 
- Si la BDPU contiene un Root Bridge _MENOR_ al almacenado, el switch comenzará a informar a sus vecinos acerca del nuevo Root Bridge.

Cuando termina el intercambio de BPDUs, el switch con _BID MÁS BAJO_ será electo Root Bridge, en la imagen siguiente: S1.


> **Importante**: recordar que el valor de BID se define por:
> - _Prioridad del bridge (`32768`, default) + VLAN ID (`1`, default) + MAC address_

![normal](../../_anexos_/Screenshot%20from%202024-01-02%2011-49-46.png)

Luego de esto pasamos a la elección de puertos designados (Ver: [STP - STA, Port roles election - Root port](STP%20-%20STA,%20Port%20roles%20election%20-%20Root%20port.md))