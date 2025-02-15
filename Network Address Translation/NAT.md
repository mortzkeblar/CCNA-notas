
NAT es un proceso diseñado específicamente para tratar el tema de [[IPv4 addressing]]  y el problema de la falta de direcciones. La traducción de direcciones reduce la necesidad de direcciones publicas IPv4 y oculta los rangos de direcciones privadas (definidos en el RFC 1918).  

> Es importante recalcar que las direcciones privadas no son enrutables en internet, por lo que si se tiene hosts con direcciones privadas se necesita tener NAT para que estos puedan salir a internet 

El proceso NAT consiste en la modificación de la dirección de origen o destino enmascarándolo generalmente en una IP publica, aunque no se limita solo a estas direcciones. 

## Cisco NAT terminology
Cuando un host en una red interna se comunica con un dispositivo en una red externa via NAT, existen cuatros direcciones involucradas en este proceso desde el _standpoint_ del router. 
- _Inside Local_ - la [[IP address]] del host interno antes del NAT
- _Inside Global_ - la [[IP address]] del host interno despues de pasar por NAT 
- _Outside Local_ - la [[IP address]] del host externo antes del NAT 
- _Outside Global_ - la [[IP address]] del host externo despues de pasar por NAT 

Estos términos tambien se pueden definir de la siguiente manera:
![[Pasted image 20250214020125.png]]

### Inside and Outside 
Los términos _inside_ / _outside_ hace referencia a las redes internas y externas respectivamente, desde la perspectiva del router. 
- _Inside address_ - es una [[IP address]] de un host ubicado en _inside network_ (red interna) del router 
	- Los hosts de un _inside network_ son llamados _inside hosts_
- _Outside address_ - es una [[IP address]] de un host ubicado en un _outside network_ (red externa) 
	- Los hosts de un _outside network_ son llamados _outside hosts_

![[Pasted image 20250213225217.png]]

### Local and Global 
Los términos _local_ / _global_ se utilizan para hacer la distinción entre las direcciones pre-natting y post-natting. 
- _Local address_ - es una [[IP address]] antes de que sea traducido por el NAT process en el router, esta dirección es el host address desde la perspectiva del _inside network_
- _Global address_ - es una [[IP address]] despues de ser traducido por el NAT process en el router, esta dirección es el host address desde la perspectiva del _outside network_ 

![[Pasted image 20250214014148.png]]

Cuando se realiza la combinación de las distinciones inside/outside y local/global es que se obtiene los cuatro términos mencionados anteriormente.

![[Pasted image 20250214015247.png]]
En este ejemplo, no se realiza el NAT del external host [[IP address]], por lo que el outside local y outside global address son iguales. Este caso no es así cuando se realiza [[DNAT]] (no se encuentra cubierto en el CCNA).

> Es importante considerar que estos términos no son estáticos en toda la red sino que dependen de la perspectiva desde donde se consideran 


## Types of NAT 
Existen multiples tipos de NAT, con respecto al CCNA los términos a considerar son:
- Static NAT - traducciones estáticas one-to-one 
- Dynamic NAT - traducciones dinámicas one-to-one 
- Dynamic PAT (Port Address Translation) - traducciones dinamicas many-to-one 




