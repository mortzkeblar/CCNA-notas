---
tags:
  - CCNA
---
Un _local route_ es una ruta hacia la IP especifica configurada en la interface del router. Al igual que una [[connected route]], estas se añaden de forma automatica en la [[routing table]]. 

En este caso la ruta usa un prefijo de tamaño `/32` ya que todos los bits forma parte de la porción de red y no pueden ser cambiados. Lo cual lo diferencia de una [[connected route]]. 

![[Pasted image 20241029073101.png|400]]

>  Una ruta local le indica al router que el paquete destinado para esa IP especifica, es en realidad para el propio router y que debe continuar con el proceso de desencapsulamiento. Una ruta local es necesaria para distinguir la propia IP configurada en el router del resto de las IP que se especifican en una [[connected route]]. 

Para poder filtrarlos en la tabla se puede usar `show ip route | include L`

![[Pasted image 20241029071920.png|500]]

