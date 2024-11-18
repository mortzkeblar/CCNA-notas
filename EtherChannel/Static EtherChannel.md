![[Pasted image 20241118031901.png]]
A diferencia de [[Dynamic EtherChannel]], en esta configuración no usamos protocolos de negociación para establecer los enlaces [[EtherChannel]]. 

Para este caso se usa la _keyword_ `on` en la que al configurarla dentro de un puerto se establece el etherchannel indendientemente de si el otro extremo esta o no configurado. Por este motivo se recomiendo no usar _static etherchannel_ ya que puede terminar causando un layer 2 loop. 

**Etherchannel mode combinations**
![[Pasted image 20241118032436.png]]