---
tags:
  - routing
  - dynamic
  - OSPF
  - protocol
  - CCNA
date created: Saturday, October 19th 2024, 11:08:14 pm
date modified: Wednesday, November 20th 2024, 11:33:50 pm
---

Open Shortest Path First (OSPF protocol) es un protocolo estándar abierto que usa algoritmos _link state_ (ver [[Interior Gateway Protocols]]) para calcular el mejor camino para llegar a una red particular. 

Desarrollado en el '98 por IETF para cubrir las necesidades y las carencias del [RIP](RIP.md). Es un protocolo más robusto y flexible que los predecesores protocolos de tipo [[Interior Gateway Protocols#Distance-Vector Protocols]] y es ideal para usarlo en entornos enterprise. 

OSPF emplea el uso de _areas_ para simplificar la administración de la red y confinar la inestabilidad de la red a un sector especifico. Además permite un amplio control sobre las actualizaciones de enrutamiento.

OSPF usa Dijkstra's Shortest Path First (SPF) algorithm, hace referencia a que es abierto ya que este algoritmo no es propietario a ninguna empresa o organización.

OSPF se considera un [[Interior Gateway Protocols]] porque es un protocolo que puede ser usado en grupos de routers que se encuentran dentro de un [AS](AS.md). 

Tiene una  [[Dynamic Routing#Administrative Distance parameter]] de 110.

Además de mejorar los metodos de protocolos anteriores, introduce caracteristicas como:
- No limitaciones en hop-count
- Rapida convergencia
- [classless](classless.md)  (permite el uso de [VLSM](VLSM.md))
- Autenticación de contraseñas
- Metodos avanzados de selección de rutas
- Etiquetado para rutas externas
- Mejor uso del ancho de banda via multicast y actualizaciones de routing periódicas.
- Permite a las redes ser divididas dentro de areas lógicas más pequeñas para la eficiencia.
- Usa direcciones multicast por eficiencia y fiabilidad en las actualizaciones de enrutamiento.
- Usa equal-cost load balancing sobre multiples rutas para optimizar el uso de ancho de banda. 
- Soporta MD5 authentication para el intercambio seguro de rutas.
- No tiene problemas de [split horizon](split%20horizon.md). 

## OSPF foundations 

> Los tres pasos fundamentales que siguen los routers OSPF para compartir la información y construir sus tablas de enrutamiento se pueden definir en:
> - Formar relaciones entre vecinos (o _neighbor relationships_) con otros routers OSPF-enabled 
> - Intercambiar información de enrutamiento para construir la _map connectivity_ de la red 
> - Calcular las mejores rutas a cada destino


[OSPF](OSPF.md) es un protocolo [[classless]] que establece relaciones con routers vecinos mediante paquetes multicast _Hello_ y temporizadores _Dead_. Las actualizaciones se realizan a través de _data structures_ llamadas _[[LSA|Link State Advertisements]]_. Estas LSA luego son organizadas en la **_Link State Database_** que actua como la tabla de topología de OSPF. Los LSAs se propagan por las [[OSPF area]]s, hasta que todos los routers tienen un mapa consistente de la red (es decir, todas las [[#Link-State Database]]s (LSDB) de los routers son iguales entre sí).

Cuando el mapa es consistente, el algoritmo SPF calcula rutas _loop-free_ (sin bucles) para cada red de destino, generando un _SPF tree_ que se refleja en la tabla de enrutamiento.

[OSPF](OSPF.md) utiliza una métrica basada en costo para determinar el camino más corto entre dos puntos.

1. Los routers OSPF envían paquetes _Hello_ por todas sus interfaces configuradas. Si los routers en el enlace los aceptan, se convierten en vecinos.
2. Algunos vecinos forman adyacencias, según el tipo de router y red (p. ej., _point-to-point_ o _broadcast_).
3. Los _Link State Advertisements_ se intercambian entre adyacencias, proporcionando información sobre enlaces, vecinos y su estado.
4. Cada router construye una base de datos _link state_ idéntica.
5. Una vez sincronizados, el algoritmo SPF calcula las mejores rutas _loop-free_ para cada destino, considerando a cada router como la raíz (_Root SPF_).
6. Finalmente, cada router genera su tabla de enrutamiento.

## Link-State Database 
En OSPF, el intercambio de información de enrutamiento se hace mediante el uso de una _data structure_ llamada [[LSA|Link State Advertisements]] (LSA). Cada router organiza los [[LSA]] que recibe en una base de datos llamada **_Link State Database (LSDB)_**.

LSDB sirve como el mapa de la topologia de la red al router, que es utilizado para calcular el _shortest path_ a cada destino de la red. Por lo cual es importante que todos los routers tengan el mismo LSDB. 

![[Pasted image 20241120002451.png]]

## OSPF configuration 
Cuando se activa [[Dynamic Routing#The `network` command]] en un interface, el router intentara formar relaciones de vecinos y compartir [[LSA]]s con otros routers conectados a esas interfaces. 

- El primer paso para configurar OSPF es crear un OSPF process mediante el comando `router ospf <process-id>`  en modo _routing config_
	- Los valores permitidos de `process-id` son de [1, 65535]
	- El valor del `process-id` es a nivel local, no tiene que hacer match necesariamente el valor con el de sus vecinos 

![[Pasted image 20241121232418.png]]

### Router ID 
En [[OSPF]], cada router tiene su propio **_router ID (RID)_**, un valor unico de 32-bits que identifica al router en el _OSPF Autonomous System_, por lo cual no puede haber dos routers con el mismo RID dentro del AS.

Para ver el _RID_ del router se puede usar `show ip protocols`.

![[Pasted image 20241121234353.png]]
El valor **RID** que toma el router se da en el siguiente orden de prioridad:
- [[#RID manual configuration]] - configuración manual con el comando `router-id <value>` en el modo _router config_ (ya debe haber un OSPF process creado)
- [[#Loopback interfaces]] - IP address más alta de una interface _Loopback_ operativa 
- IP address más alta de una interface física operativa 

#### Loopback interfaces
> Es importante recalcar que una _Loopback interface_ no es el mismo concepto que una _Loopback address_, esta ultima hace referencia al segmento reservado `127.0.0.0/8` que son usados por los dispositivos para enviar mensajes a si mismos bajo el concepto de "looped back".

Un interface _loopback_ es una **_virtual router interface_** que siempre esta activa mientras el dispositivo este operativo (o sea deshabilitado mediante `shutdown` comando). 
- Las interfaces _loopback_ no depende del puertos fisicos sino que son enteramente _software-based_

> La ventaja de una interface _loopback_ es que es confiable para usar como identificador ya que siempre esta activa y no depende de una interface fisica que en determinado momento puede caer por cualquier motivo  


![[Pasted image 20241121235956.png]]
En la imagen se puede apreciar la ventaja de tener una interface _loopback_ para establecer una conexión SSH incluso cuando alguna interface cualquiera pueda fallar. 

Por tal motivo, es **recomendable** configurar interfaces _loopback_ que actuen como _RID_ para que todos los routers OSPF se puede comunicar mediante sus interfaces _loopback_. 

Para configurar una interface loopback se usa el comando `interface loop-back <number>`, el número más comun a usar es $0$. Por lo que la interface se nombra  como `loopback0`.

![[Pasted image 20241122000829.png]]

> Es un convención configurar las interfaces _loopback_ con un /32 netmask, ya que estas interfaces no necesitan comunicarse con otros dispositivos en su misma subred por lo que no necesita tener un rango de IPs. 

Para que el cambio de **RID** se haga efectivo con si se quiere usar la interface _loopback_ se debe ejecutar el comando `clear ip ospf process` en el modo _privileged EXEC_.

> Aun así el cambio de RID no sera aplicado en el router, se recomienda que si se usa la _loopback_ interface como RID esta sea configurada antes que el proceso OSPF, asi toma esta dirección primero por el orden de prioridad.  

#### RID manual configuration 
El metodo recomendado para configurar el **_router ID_** es setear manualmente el valor mediante el comando `router-id <router-id>`.
- Si es que el router ya tiene una _neighbor relationships_ con sus vecinos, se debe restablecer el proceso OSPF mediante el comando `clear ip ospf process` para que el nuevo RID tenga efecto

![[Pasted image 20241122002916.png]]

> Es importante recalcar que un RID no es una dirección IP, aunque tenga la misma notación de octetos. Se debe ver solo como un valor unico de 32-bits en el OSPF _Autonomous System_


### OSPF-enable on interfaces 
Esta parte es en la que el router puede comenzar a hacer OSPF _neighbor_ y compartir la información de enrutamiento. Se trato anteriormente en [[Dynamic Routing#The `network` command]] que es el metodo para activar OSPF en una interface. 

![[Pasted image 20241122004941.png]]

![[Pasted image 20241121235956.png]]

Tambien existe otro metodo **más directo** para habilitar OSPF en las interfaces mediante el comando `ip ospf <process-id> area <area>` en el modo _interface config_. Esta forma permite especificar las interfaces donde se quiere activar [[OSPF]].

![[Pasted image 20241122011645.png]]

### Passive interface 
Existen casos en los que se desea anunciar un _network prefix_ sin establecer OSPF _neighbor relationships_. 
- Para formar _[[OSPF]] neighbor relationships_, los routers envian mensajes _hello_ fuera de sus interfaces. Pero puede haber el caso de que del otro lado no este necesariamente un OSPF-router, por ejemplo puede haber un host final. 
	- Enviar OSPF mensajes innecesariamente tambien implica un riesgo de seguridad

Una [[passive interface]] se define como una interface que NO envia mensajes OSPF para formar _neighbor relationships_, incluso cuando OSPF este habilitado en esa interface. Aun así, el router va a anuncar el _network prefix_ de esa interface a los OSPF vecinos. 

![[Pasted image 20241122012825.png]]

Es una buena practica configurar las interface _loopback_ como passive, ya que es un interface virtual que no esta físicamente conectada a ninguna red.  

Para configurar una _passive interface_ se usa el comando `passive-interface <interface>` en el modo _router config_. 

![[Pasted image 20241122013214.png]]

Otro método para configurar interfaces pasivas es mediante el comando `passive-interface default` que hace que todas las interfaces sean pasivas por defecto. 

### Advertising a default route
En OSPF se puede configurar una [[Default route]] para comunicarse con redes externas como internet. 

En este caso, se debe crear una [[Default route]] en el router de borde (que esta conectado con la red externa, ISP i. e.). Luego en ese router se debe usar el comando `default-information originate` en el router de borde para que el default route sea distribuido entre todos los router OSPF y estas van a agregar esa _default route_ es su [[Routing Table]].  

> Es importante destacar que si se usa `default-information originate` en un router que no tiene una _default route_, esta no anunciara una _default route_ a los demas routers. 

![[Pasted image 20241122014419.png]]

Al ejecutar `show ip protocols` se puede ver que R1 ahora se categorizo como _Autonomous System Boundary Router (ASBR)_. 



## connected

- [OSPF terminology](OSPF%20terminology.md)
- [OSPF explain summary - advanced](OSPF%20explain%20summary%20-%20advanced.md) 
- [OSPF network types and neighbors](OSPF%20network%20types%20and%20neighbors.md) 
- [OSPF routes](OSPF%20routes.md) 
- [(legacy) OSPF router types]((legacy)%20OSPF%20router%20types.md) 
- [LSA](LSA.md) 

#TODO ver este contenido y resumir: https://itskillbuilding.com/networking/network/ospf/ospf-protocol/#tab-3711