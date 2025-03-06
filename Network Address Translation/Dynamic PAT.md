**_Dynamic Port Address Translation_**, es un tipo de [[NAT]] que realiza la traducción de direcciones de forma _many-to-one_, mappeando direcciones _inside local_ a direcciones _Inside global addresses_ mediante puertos [[TCP]]/[[UDP]]. 

> Al usar un puerto único para cada _communication session_ esto permite una única [[IP address]] pública pueda ser compartida por multiples inside hosts, tambien permite realizar el tracking de las conexiones a partir del puerto donde se hace el mapping de la sesión.  

Con _Dynamic PAT_, los direcciones que se pueden traducir con una sola [[IP address]] es de $65536$  o $2^{16}$ direcciones, aunque tambien depende de otros factores como la capacidad de procesamiento del router. 

## Dynamic PAT using a pool 
Este metodo implica crear un pool para las direcciones externas, lo cual permite asociar más de una [[IP address]]. 

> La keyboard central para especificar un Dynamic PAT es `overload`, incluso otro de los nombres por el que se conoce a este tipo de NAT es _NAT overload_ .

![[Pasted image 20250306001306.png]]

- Al traducirse un paquete los puertos **pre** y **post** traducción se tratan de mantener iguales si es posible, caso contrario se usa otro puerto disponible por donde se realiza el translate NAT. 
- Las direcciones externas que se utilizan en el pool para el translate NAT, van a ser usadas de forma escalonada. Es decir, se va usar una dirección hasta que no queden puertos disponibles, para luego pasar a usar la siguiente dirección en el pool. 

![[Pasted image 20250306002335.png]]

- Al recibir la respuesta del paquete, el router es capaz de identificar a cual sesión pertenecen los paquetes debido a que los puertos usados para el translate NAT (en la IP pública) son únicos. 

![[Pasted image 20250306004518.png]]

## Dynamic PAT using an Interface IP's address 

Este metodo resulta útil cuando se quiere realizar _dynamic PAT_ a través de una única [[IP address]] que ya este asociada con una interface (generalmente la WAN del ISP). 

![[Pasted image 20250306004928.png]]

Para esto se deben usar los siguientes comandos: 

![[Pasted image 20250306005037.png]]

Las configuraciones a definir son casí iguales al configurar un [[Dynamic NAT]], la diferencia radica en el comando `ip nat inside source list <acl-list> interface <interface> overload`, donde especifica la [[IP address]] de la interface que debe ser usada para realizar PAT. 
- En este caso, el keyboard `overload` es opcional

Como siempre, para verificar las traducciones que se realizan se puede usar `show ip nat translations`

![[Pasted image 20250306014915.png]]
Otro comando útil para la verificación de [[NAT]] es `show ip nat statistics`

![[Pasted image 20250306014947.png]]

