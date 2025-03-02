_Dynamic NAT_ es un tipo de [[NAT]] similiar a [[Static NAT]] en el sentido que realiza el mapping one-to-one de los _inside local adresses_ hacia _global addresses_, la diferencia radica en la asignación del mapping. 

Como su nombre indica, cuando se realiza _dynamic NAT_ el router hace mapping dinamicamente en la asignación de direcciones desde un _pool address_. 

![[Pasted image 20250227071646.png]]

La configuración consiste en lo siguiente: 
- Definir un rango de _inside local (private) addresses_ mediante un [[Project/Networking/CCNA-notas/Security/ACL/ACL|ACL]] 
- Definir un rango de _inside global (public) addresses_ mediante un [[NAT]] pool 
- Mappear el [[Project/Networking/CCNA-notas/Security/ACL/ACL|ACL]] hacia el [[NAT]] pool 

![[Pasted image 20250227071906.png]]


- Se especifican las interfaces donde se encuentran conectados la `inside` network y la `outside` network 
- Creamos un ACL para identificar el rango de direcciones `inside local` a ser traducidos
	- Considerar que en este caso, el ACL no se usa para ver cuales paquetes reenviar y cuales bloquear, sino que indica cuales paquetes deben pasar por [[NAT]] translation  
		- Los rangos que esta permitidos pasaran a ser traducidos 
		- Los rangos que entren en `deny` se reenviaran sin la traducción, el cual probablemente sera droppeado por el ISP al ser una private address 
- Luego especificamos un rango para las _inside global addresses_, que son las direcciones IPv4 públicas que seran usadas para la comunicación externa. Se crea con `ip nat pool <start-ip> <end-ip> prefix-length <length>`
	- El `prefix-length` debe coincidir con el rango que se indica, caso contrario el comando sera rechazado. Tambien se puede usar la `netmask` keyboard indicando la mascara de red. 
- El paso final consiste en la combinación del ACL con el pool mediante un _NAT statement_, consiste en el mapping del rango de _inside local addresses_ contenidos en el ACL hacia el rango de _inside global addresses_ contenidos en el pool, con el comando `ip nat inside source list <acl> pool <pool>`

Es importante considerar que el mapping en _dynamic NAT_ es de forma **one-to-one**, lo que resulta en una gran limitación ya que la cantidad de direcciones que pueden ser traducidas es equivalente a la cantidad de direcciones disponibles para la conexión en el pool.

![[Pasted image 20250301023022.png]]

Si una dirección tiene que ser traducida pero ya no hay direcciones disponibles en el pool, este sera droppeado, incluso a pesar de que cumpla con la condición del ACL. 
- Tambien observar que los paquetes que sean denegados por el [[Project/Networking/CCNA-notas/Security/ACL/ACL|ACL]] seran reenviados igualmente pero no pasaran por el proceso de traducción NAT. 
