_Spannign Tree Protocol (STP)_, es un protocolo (habilitado por defecto en [[switch]]es Cisco) que ayuda a resolver problemas de tipo _Layer 2 loops_ dentro de una [[LAN]], estos son frames que estan en loop dentro de la red de forma indefinida. 

> El problema que ataca STP es similar a [[TTL]] en la [[IPv4 Header]]. 

Este problema suele aparecer principalmente cuando se trata de introducir conexiones redundantes en la red. Se puede identificar dos problemas principales: _broadcast storn_ y _MAC address flapping_.

![[Pasted image 20241110201000.png]]

- *Broadcast storn* - se conoce como tormenta broadcast cuando la cantidad de frames que hacen loop en la red. Comienzan a saturar al punto de consumir muchos recursos como CPU o ancho de banda haciendo que la red se inutilizable.
- *MAC address flapping* - este problema ocurre cuando un [[switch]] aprende la misma MAC address desde dos puertos diferentes. 

Spannign Tree Protocol se puede resumir como _la prevención de Layer 2 Loops mediante el bloqueo de la conexiones redundantes  a nivel de  la topologia LÓGICA (no FÍSICA), manteniendo una unica ruta activa entre dos nodos cualquieras en un una [[LAN]]_. 

![[Pasted image 20241110205531.png|500]]

## STP algorithm 
Este es el proceso para crear un topologia loop-free, los pasos de forma summarized consisten en:
- Root bridge election (uno por LAN)
	- Lowest BID
- Root port selection (uno por switch, excluyendo al root bridge)
	- Lowest root cost 
	- Lowest neighbor BID 
	- Lowest neighbor port ID
- Designated port selection (uno por segmento)
	- Port on the switch with the lowest root cost 
	- Port on the switch with the lowest BID 

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
Una vez elegido el _root bridge_, los switches non-root elegin uno de sus puertos como _root port_.
- El _root port_ es el puerto con la mejor ruta para llegar al _root bridge_
	- Este se calcula en base a la información recibida de los BPDUs vecinos. Los principales parametros para la selección del root port son:
		- Lowest root cost 
		- Lowest neighbor BID 
		- Lowest neighbor port BID

El _root cost_ de un puerto es el valor que indica que tan eficiente es la ruta por ese puerto al _root bridge_. Un valor menor es mejor, cada puerto tiene asociado un _cost_ asociado.

**STP port cost values**
![[Pasted image 20241112003701.png]]
El root cost de un puerto es el costo total de los puertos que conducen hasta el root bridge. 
- El puerto con el root cost más bajo se convierte en el _root port_.
	- Si hay más de puerto con el root cost más bajo, el puerto conectado al vecino con el BID más bajo se convierte en el _root port_
		- Si dos o más puertos tiene el mismo root cost y estan conectados al mismo vecino, el puerto conectado al puerto del switch vecino con el _port ID_ más bajo se convierte en el _root port_

![[Pasted image 20241112015733.png]]
- SW4 selecciona a G0/0 como root port ya que tiene el root cost más bajo de entre todos sus puertos 
- Para SW2, tanto G0/0 como G0/1 tiene el mismo root cost, este selecciona a G0/1 como root port ya que esta conectado a SW1 que tiene el BID más bajo
- Para SW1, tanto G0/0 como G0/1 tienen el mismo root cost y como ambos puertos estan conectado el root bridge SW3, no sirve usar el BID más bajo como desempate ya que en ambos casos son iguales. El desempate es a favor de G0/1 porque esta conectado al puerto vecino con el _port ID_ más bajo


#### Lowest root cost 

> Una vez que se elige un _root bridge_ solo este envia mensajes BPDUs que reciben los otros switches non-root y estos los reenvian a sus vecinos con las actualizaciones necesarias 

Cuando el _root bridge_ envia mensajes BPDU, el valor del _root cost_ es igual a $0$. Cuando el mensaje BPDU pasa por un switch non-root este lo reenvia sumando el _port cost_ en el que recibieron las BPDU. 

![[Pasted image 20241113043326.png]]

En la imagen anterior, vemos que solo SW4 puede elegir su _root port_ en base al _root cost_ más bajo. Para SW1 y SW2, se necesita escalar al siguiente desempate: _lowest neighbor BID_.

#### Lowest neighbor bridge BID 
La BID del switch se envia dentro de la BPDU, la cual se usa para el desempate cuando hay más de un _lowest root cost_. En este caso el puerto conectado al switch vecino con el BID más bajo es elegido como _root port_.

![[Pasted image 20241113045121.png]]

> El puerto vecino conectado al _root port_ de un switch es llamado **designated port (forwarding)**. 
> 
> Como el root port proporciona al switch una unica ruta hacia el root bridge, se debe asegurar que el vecino no bloquee ese enlace. 

#### Lowest neighbor port ID 
Este es el desempate final al que se llega para la selección de un _root port_. 

El port ID es un identificador unico (que corresponde al puerto vecino el cual envio el BPDU) para cada puerto en un [[switch]], contiene un valor de prioridad configurable (128 por defecto) seguido de un valor númerico secuencial al puerto. Para ver sus valores se puede usar `show spanning-tree`.

![[Pasted image 20241113050009.png]]

Para modificar el valor de prioridad se puede usar `spanning-tree vlan <vlan-id> port-priority <priority-value>`

> Es importante remarcar que los valores de port ID que se usan para el desempate corresponden a los port ID de los vecinos, no los valores de los puertos propios.

![[Pasted image 20241113050308.png]]

### Designated port selection 
Luego de que se hayan definido los _root ports_ para los switches non-root, el paso final es designar los _designated ports_.
- Un root port es un puerto que reenvia la trafico apuntando hacia el root bridge 
- Un designated port es un puerto que reenvia el trafico apuntando hacia afuera del root bridge
	- El root bridge solo tiene designated ports, ya que todos apuntan hacia fuera del root bridge 

Se considera que debe haber un _port designated_ por cada _segment_ en la LAN. En el contexto de STP definimos segmento como el enlace entre [[switch]]es. Los designated ports son elegidos según los siguientes criterios (en orden de prioridad)
1. El puerto en el [[switch]] con el _lowest root cost_ es designated 
2. El puerto en el [[switch]] con el _lowest BID_ es designated 

En los pasos anteriores ya se habian establecido designated ports, tanto en la elección del root bridge (todos sus puertos son designated) como en la selección de los root ports (los puertos conectados a un root port son designated ports). 

En este paso se debe definir los designated ports para los segmentos/enlaces restantes, por cada segmento solo puede haber un designated port. Lo que implica que el otro extremo del segmento queda como _non-designated port_ y estos se mantienen en estado **blocking**. 

![[Pasted image 20241114005829.png]]

#### Port on the switch with lowest root cost 
Este es el primer parametro usado para seleccionar un designated port, es importante recalcar que cuando hace referencia al _lowest root cost_ esta hablando sobre las comparación de los valores root cost correspondientes al _root port_ del [[switch]], no sobre los valores de los puertos sobre los que se esta decidiendo. 

![[Pasted image 20241114011523.png]]

#### Port on switch with lowest Bridge ID 
El desempate definitivo para seleccionar el designated port y consiste en la comparación entre los BID de los switches, el puerto en el switch con el BID más bajo es seleccionado como _designated port_. 

![[Pasted image 20241114012324.png]]


