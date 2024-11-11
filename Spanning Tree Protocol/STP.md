_Spannign Tree Protocol (STP)_, es un protocolo (habilitado por defecto en [[switch]]es Cisco) que ayuda a resolver problemas de tipo _Layer 2 loops_ dentro de una [[LAN]], estos son frames que estan en loop dentro de la red de forma indefinida. 

> El problema que ataca STP es similar a [[TTL]] en la [[IPv4 Header]]. 

Este problema suele aparecer principalmente cuando se trata de introducir conexiones redundantes en la red. Se puede identificar dos problemas principales: _broadcast storn_ y _MAC address flapping_.

![[Pasted image 20241110201000.png]]

- *Broadcast storn* - se conoce como tormenta broadcast cuando la cantidad de frames que hacen loop en la red. Comienzan a saturar al punto de consumir muchos recursos como CPU o ancho de banda haciendo que la red se inutilizable.
- *MAC address flapping* - este problema ocurre cuando un [[switch]] aprende la misma MAC address desde dos puertos diferentes. 

Spannign Tree Protocol se puede resumir como _la prevención de Layer 2 Loops mediante el bloqueo de la conexiones redundantes  a nivel de  la topologia LÓGICA (no FÍSICA), manteniendo una unica ruta activa entre dos nodos cualquieras en un una [[LAN]]_. 

![[Pasted image 20241110205531.png|500]]

## STP algorithm 
Este es el proceso para crear un topologia loop-free, los pasos consisten en:
- Root bridge election
- Root port selection 
- Designated port selection 

![[Pasted image 20241110213336.png|500]]

### Root bridge election 

Primeramente STP elige un [[switch]] para como _root bridge_ para la LAN. Un root bridge es el punto central de referencia para la topología STP. 

> Todos los switches en la red tienen garantizado una ruta activa hacia el root bridge 

> En STP se puede tomar como equivalentes _bridge_ y _switch_.

- La elección del _root bridge_ se lleva a cabo con los switches que comparten el _STP Bridge Protocol Data Unit (BPDU)_ entre sí
	- Los BPDU se usa para tomar todas las decisiones en el STP algorithm, no solo para la elección del root bridge 
- Los mensajes BPDU se envian cada 2 segundos, entre los datos enviados. Los campos importantes para la elección del root bridge son 
	- My _Bridge Identifier (BID)_, el número de identificación unico propio del switch que envia el BPDU en la [[LAN]]
	- El _BID_ del switch que creen que es el root bridge 
		- Al iniciar un [[switch]], todos se identifican asi mismos como el root bridge
			- Esto significa que el [[switch]] al iniciarse, pone su propio BID en el campo del BID del que cree que es el root bridge

#### Bridge Identifier (BID)
El switch con el BID superior es el elegido como root bridge de la [[LAN]], este criterio se basa en los parametros que considere el STP algorithm. 
- El bridge root elegido implica que envio el BPDU con el campo _My BID_ numericamente más bajo

El BID (o Bridge ID) es un número de 64 bits que consiste de 16 bits como _bridge priority_ y los restantes 48 bits como la _MAC address_.
- _Bridge priority_ - esta se divide en dos partes, el valor del bridge priority es la suma de los dos campos que la componen. $Bridge\ priority=Priority + Extended\ System\ ID\ (VLANID)$
	- _Priority_, por defecto tiene el bit más significativo en 1 (esto equivale al valor 0d32768).
	- _Extended System ID (VLAN ID)_, el valor de este campo es igual al de la VLAN ID 

![[Pasted image 20241110225637.png]]

Los switches Cisco corren una versión propietaria de STP llamada **Per-VLAN Spanning Tree Plus (PVST+)**. PVST+ crea diferentes instancias de STP por cada VLAN, esto es util ya que se puede realizar un balanceo en el trafico, ya que se puede elegir los enlaces a deshabilitar según la VLAN.

![[Pasted image 20241110232819.png|400]]

> Como la VLAN ID forma parte del BID, es significa que el campo _bridge priority_ es unico para cada instancia STP. 
> - Esto implica tambien que cada BID es unico para cada bridge/switch ya que el BID se compone de $Bridge\ Priority + MAC\ Address$ (el valor es resultado de una concatenación, no una suma)


#### Comparing BIDs 
A pesar de que pueda parecer obvio, la MAC address que se agrega en el BID no es ninguno perteneciente a alguno de los puertos del switch. Esta MAC address es una dirección aparte que se usa para identificar al switch en su conjunto, esta dirección es la que se usa para el desempate de BID en la elección del _root bridge_. 

Para encontrar el BID con el valor más bajo, se debe comparar los digitos más significativos (de izquierda a derecha) y ver cual es el menor entre todos. 

- SW1: 32769:5254.000f.adab
- SW2: 32769:5254.0013.cf9a
- SW3: 32769:5254.0016.5d5e
- SW4: 32769:5254.001d.d23a

Para los siguientes valores, la parte `32769:5254.00` es igual para todos. Pero SW1 tiene el siguiente digito en $0$ mientras que los demás lo tiene en $1$. Esto es suficiente para saber que SW1 tiene el BID con el valor más bajo. En consecuencia, SW1 es elegido como _root bridge_.

Esto se puede verificar esto usando `show spanning-tree`. 

![[Pasted image 20241111015937.png]]

> El switch con el segundo BID más bajo, se le asigna el rol de _secondary root bridge_


#### Configuring the Bridge Priority 
Para poder elegir un _root bridge_, podemos modificar el campo _bridge priority_ (por defecto tiene el valor $32768$). En general se usa esta forma si necesitamos tener la ruta más optima para llegar a un switch especifico, por ejemplo si este esta conectado al router que da acceso a redes externas.

Para configurar el bridge priority se hace uso del comando `spanning-tree vlan <vlan-id> priority <priority>`. 

![[Pasted image 20241111021330.png]]

Recordar que el valor priority esta limitado a valores especificos ya que se encuentra dentro de un campo de 16 bits, de las cuales solo tiene poder sobre los primeros 4 bits más significativos. 

Otra forma para configurar el bridge priority es mediante el uso del comando `spanning-tree vlan <vlan-id> root {primary | secondary}`
- `secondary`, el valor se configura en $24576$ (se resta $4096$ por debajo del valor default)
- `primary`, el valor se establece según:
	- Se configura en $24576$ (se resta $4096$ dos veces por debajo del valor default)
	- Si $24576$ no alcanza, configura el priority con el valor más alto que sea multiplo de 4096 pero menor al valor de BID más bajo en ese momento. 

Se desaconseja el uso de los valores `primary | secondary` al configurar un valor priority.
- El valor que se establece para el `secondary` root bridge es arbitrario y no verifica que pueda haber un BID que sea el segundo más bajo que el valor establecido 
- No se puede establecer el priority en $0$ mediante este metodo, lo cual hace que si tienes un priority de $4096$ y luego usa `primary` command, este falle. 

![[Pasted image 20241111023420.png]]
> Para asegurar la asignación de un switch como root bridge, se recomienda usar el valor $0$ como priority, con el comando `spanning-tree vlan <vlan-id> priority 0`. 

### Root port selection







