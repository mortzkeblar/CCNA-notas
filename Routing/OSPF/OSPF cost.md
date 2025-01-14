---
tags:
  - routing
  - OSPF
  - CCNA
date created: Saturday, October 19th 2024, 11:08:14 pm
date modified: Wednesday, November 20th 2024, 11:33:50 pm
---
En [[OSPF]], el [[Dynamic Routing#Metric parameter]] es llamado **_cost_**. El _costo_ es el valor que OSPF usa para determinar la mejor ruta a cada destino (el _shortest_ en OSPF). 

Cada interface OSPF-enabled tiene asociado un valor de costo, el costo de un ruta es la suma de todos los costos de las interfaces por donde un paquete es enviado (out). 

![[Pasted image 20241121012304.png]]

Si bien la definición anterior es correcta, es más preciso hacer referencia al **_costo del enlace_**, ya que en ambos extremos del enlace el valor de costo debe ser igual y en el proceso del calculo solo se hace uso del costo de uno de los extremos (o interface). 
- Si el el costo en ambos extremos de un enlace son diferentes, se considera una mala configuración de OSPF. 
- De esto se deduce que el valor de costo para calcular una ruta es igual en ambos sentidos (en la imagen, el costo de llegar de R1 a R3 es igual al costo ir desde R3 a R1)


## Reference bandwidth 

 El costo de un enlace se calcula un valor de _reference bandwidth_ por el _bandwidth_ (o ancho de banda) del enlace en cuestión, el _reference bandwidth_ por defecto es de 100 Mbps y la formula queda como  $100/bandwidth$

> Si el valor resultante del calculo del costo es menor a uno, el valor del costo se redondea a $1$ porque OSPF no aceptar valores fraccionarios o decimales como costo

| Link       | Cost $100/bandwidth$ |
| ---------- | -------------------- |
| 10 Mbps    | 10 (100/10)          |
| 100 Mbps   | 1 (100/100)          |
| 1000 Mbps  | 1 (100/1000 = 0.1)   |
| 10000 Mbps | 1 (100/1000 = 0.01)  |

El problema con el _reference bandwidth_ por defecto, es que todos los enlaces mayores o iguales a 100 Mbps tiene el mismo valor de costo. Lo cual hace que calcule rutas subóptimas.

![[Pasted image 20241121020747.png]]

En la imagen se puede ver que desde R1, para llegar a la red `10.1.3.0/24`. Si bien la ruta a la red via R2 es superior, al tener ambas rutas el mismo costo. R1 va a agregar ambas rutas a al [[Routing Table]] para luego realizar load-balancing mediante [[Dynamic Routing#ECMP]]. 

> El hecho de usar [[Dynamic Routing#ECMP]] sobre dos enlaces con diferente _bandwidth_ puede provocar que el enlace FastEthernet se sature mientras el enlace GigabitEthernet es infrautilizado, por lo que no es recomendable el uso. 

Para cambiar el valor _reference bandwidth_ se usa el comando `auto-cost reference-bandwidth <Mbps>` en modo _router config_ (se recomienda setear el valor del enlace más alto que existe en la red). El valor configurado se puede ver con `show ip ospf interface brief`.

![[Pasted image 20241121025919.png]]

> Es importante asegurar que si se realiza un cambio en el _reference bandwidth_, estos deben estar aplicados en todos los routers [[OSPF]] para asegurar una [[Dynamic Routing#Route selection]] consistente y a fin de evitar [[routing loops]].
> 

## Modifying the cost of a link 
Existen dos metodos para poder modificar el costo del enlace.
- Configurar un valor de costo directamente con el comando `ip ospf cost <cost>` en el modo _interface config_
- Modificar el valor _bandwidth_ de la interface con el comando `bandwidth <kbps>` en el modo _interface config_
	- Es importante no confundir el valor de _bandwidth_ con el de _reference bandwidth_.
	- A diferencia del primer metodo, esto no modifica el valor de costo directamente sino que influye en el calculo del costo mediante la formula  $reference\ bandwidth/bandwidth$ 
	- Modificar el _bandwidth_ de la interface tambien puede afectar otras funciones como el QoS, por lo cual no es el metodo primario recomendable a usar 

> El comando `bandwidth <kbps>` solo afecta a las operaciones de los protocolos de enrutamiendo como OSPF, EIGRP, etc. No afecta a la forma en como opera la interface, para modificar la velocidad de funcionamiento en la interface se debe usar el `speeed` comando. 






















