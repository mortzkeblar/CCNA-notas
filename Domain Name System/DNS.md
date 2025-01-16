**Domain Name System (DNS)** es un sistema de nombres jerárquico que permite la comunicación entre dispositivos en una red. Traduce nombres de dominio que puede ser legibles para humanos a direcciones de protocolos de internet, en general IP. 

### Uniform resource locators 
Un **Uniform Resource Identifier (URI)** es una secuencia unica de caracteres que identifica un recurso (no se limita a recursos informaticos). Los URIs se clasifican en:
- _Uniform Resource Names (URNs)_ - identificador unico de un recurso particular, que no indica su ubicación o como acceder a él (ie libros identificados por ISBN)
- _Uniform Resource Locators (URLs)_ - identificador unico de un recurso particular, identifica tanto al propio recurso así como la forma de encontrarlo (ie. dirección web)

Una **URL** consiste de multiples elementos, como puede ser un _scheme_, _authority_ y _path_. 
- El scheme le indica al navegador el protocolo que debe usar para enviar una solicitud a la web server 
- El authority indica el nombre del servidor web, en este caso un _domain name_
- El path identifica el recurso especifico en el server 

![[Pasted image 20250115235913.png]]

### Name resolution
Luego de presionar `Enter` en el cuadro de busqueda:
- El dispositivo revisa en su _DNS cache_ si tiene una entrada que haga matching
	- DNS cache almacena de forma temporal nombres que ya fueron resueltos, estos existen en diferentes nivales. Tanto a nivel de SO como las diferentes aplicaciones que se usan
- Si existe una entrada en el DNS cache, este ya tiene la dirección del servidor. Por lo necesita solicitarlo a un DNS server 
- En caso de que no este en el DNS cache, este envia un petición a un DNS server para poder resolver el nombre a una [[IP address]] 
- Una vez se haya resolvido el nombre a una IP, esta se almacena en el cache del dispositivo para no tener necesidad de enviar nuevas peticiones al DNS server

![[Pasted image 20250116000711.png]]

> Las DNS queries son enviadas al puerto 53 del DNS server, se puede usar tanto [[TCP]] como [[UDP]] 

#### The `hosts` file 
Antes de que se realice un DNS request, el dispositivo revisa el archivo `host` (`C:\Windows\System32\drivers\etc`) por si ya existe una asignación para lo solicitado. Anterior a DNS, este era la forma en que se realizaba la resolución de nombres, pero como se puede ver, no es escalable a comparación de DNS.

## DNS hierachy
DNS es un _hierarchical naming system_ para la computadores en internet. Esta se organiza sobre una estructura _tree-like_. Un dominio es una subarbol o _subtree_ de esa estructura, bajo el control administrativo de un ente particular.

![[Pasted image 20250116003134.png]]
- Todos los dominios se encuentra debajo del _root domain_ (representado por un `.`)
- Debajo del root domain, se ubican los _top-level domains (TLDs)_
- Debajo de los top-level domains, se encuentran los _second-level domain (SCDs)_
- Debajo de los second-level domains, se encuentra los _third-level domains_, _fourth-level domains_, etc 

> Cada dominio es un subdominio de los dominios que se encuentra por encima en la jerarquía 

#### `www` domain name
El dominio `www` como se puede ver en la imagen, no representa toda la web sino que es una convención usada para denominar a un servidor que aloja un sitio web. No representa una technical standard, tambien se puede observar que muchos sitios no usan `www` o bien usan un redireccionamiento hacia una mismo sitio web pero sin el `www` por delate 

El nombre completo de un host en internet se llama **fully qualified domain name (FQDN)**. 

Un FQDN se escribe con un punto que separa cada parte (i.e. `www.google.com.ar.`). Se puede observar que el ejemplo contiene un punto final, lo cual es valido para que se considere un FQDN, si no tiene un punto final es llamado un **partially qualified domain name (PQDN)**. 

> El punto final no indica el _root domain_ sino que es un separador entre el TLD y el root domain, que no puede ser representado. Por simplificación se considera a un FQDN un host sin el punto final. 

#### Domain name and hostname 
- _Domain name_ - se refiere a una región de control administrativo dentro de la jerarquía DNS, esta incluye cualquier subdominio por debajo de él. Tambien puede especificar un nodo especifico dentro del dominio (i.e. `srv1.example.com`)
	- Un mismo dominio puede tener varios significados. `google.com` se refiere al ámbito del control administrativo de Google dentro de la jerarquía DNS pero tambien es un servidor web particular.
- _Hostname_ - es un identificador para un dispositivo especifico en la red. Este puede ser un nombre simple así como un FQDN


## Recursive and iterative lookups
Para la resolución de nombres tenemos dos tipos de DNS lookups: _recursive_ y _iterative_.

![[Pasted image 20250116052824.png]]
1. PC1 intenta acceder a `mail.google.com`
2. PC1 envia un recursive DNS query a su servidor DNS configurado 8.8.8.8
	1. Un _recursive DNS query_ le pide al DNS server que le de una respuesta definitiva, ya sea la [[IP address]] o un error de resolución 
	2. A partir de áca se generan DNS queries producidas desde 8.8.8.8 hacia otros DNS servers para encontrar la respuesta que pide PC1
		1. Para realizar esto, el server genera las consultas en la cima de la jerarquía 
3. 8.8.8.8 envia un _iterative DNS query_ al _root DNS server_, este es un server que se encuentra en la cima de la jerarquía DNS. 8.8.8.8 le va a pedir al root server que resuelva el dominio con una IP
4. _Root DNS server_ va a responder a 8.8.8.8 con una referencia a otro DNS server. En este caso a un TLD server.
	1. Este proceso se ejecuta de la misma forma hasta que uno de los servidores DNS responda con la resolución del nombre y le indique cual es la IP que busca 
5. 8.8.8.8 envia un _iterative DNS query_ al TLD server, este server desconoce cual es la IP asociada a `mail.google.com` 
6. El TLD server responde con una referencia a otro DNS server, en este caso el _authoritative DNS server_ para el dominio `google.com`
	1. Una _authoritative DNS server_ mantiene un conjunto de registros para un dominio especifico por lo que es capaz de dar una respuesta definitiva sobre el dominio que esta buscando 
7. 8.8.8.8 envia una query al _authoritative DNS server_ 
8. 8.8.8.8 recibe la respuesta con el resolución de IP por el _authoritative DNS server_
9. 8.8.8.8 finalmente responde a la recursive query de PC1
10. PC1 inicia la comunicación con `mail.google.com`

> 8.8.8.8 es un servidor publico de Google de tipo _recursive resolver_, un DNS server que resuelve las queries enviadas por los clientes

## DNS record types 
En DNS existen varios registros, más alla de los que asocian un nombre con una [[IP address]] especifica.

![[Pasted image 20250116060711.png]]

#### DNS propagation delay 
_DNS propagation delay_ es el tiempo que toma para que se realicen los cambios en los registros DNS a través de internet, este puede demorar desde unos minutos hasta 72 horas. 

Se tiene un valor _Time to live (TTL)_ (no es igual al [[TTL]] de IPv4) para los registros DNS que indica el tiempo que deberian almacenar el registro en cache los DNS servers, un TTL más bajo implica que el registro se actualiza más frecuentemente. Lo que puede ser util si se desea realizar cambios en los registros y se espera que los servers actualicen el cambio lo antes posible.

## DNS in Cisco IOS 
Los dispositivos Cisco tiene la posibilidad de ser configurados tanto como clientes como servidores DNS. 
![[Pasted image 20250116072341.png]]
### Cisco IOS as a DNS client
Primeramente, se debe habilitar al equipo para que pueda realizar _DNS queries_, ya sea como cliente o como un DNS server que consulta a otros DNS servers, esto se logra con `ip domain lookup` (habilitado por defecto).

El segundo comando es `ip name-server <ip-address>`, donde `ip-address` es la dirección del servidor DNS al que se realizara las consultas. Tambien se puede especificar multiples DNS servers con `ip name-server <ip-address> <ip-address-2> <ip-address-3>`

![[Pasted image 20250116073507.png]]

### Cisco IOS as a DNS server 
Una ventaja de tener un DNS server local, es que se gana rendimiento al realizar consultas recurrentes a ciertos sitios. Estos ya se encuentra almacenados en cache, por lo que no necesita salir a buscarlo a redes externas.

Para configurar un DNS server debe usar `ip dns server`, una vez configurado el router ya tiene la capacidad de responder los _DNS queries_ de los clientes. Evidentemente el router por si mismo no es capaz de resolver direcciones externas por lo que se debe estar antes configurado `ip name-server <ip-address>`.

![[Pasted image 20250116074252.png]]
Es importante aclarar que ambas _DNS queries_, tanto la de PC1 como la R1 hacia 8.8.88 son _recursive queries_. 8.8.8.8 es el que realiza los _iterative queries_ para tratar de resolver el domain name. El rol de R1 es el de reenviar las DNS queries de sus clientes y almacenar en cache las respuestas. 

Tambien es posible mappear name-to-ipaddress de forma manual en el router, para la comunicación interna.

![[Pasted image 20250116075010.png]]

`ip domain name <domain>` permite definir un dominio por defecto, que el router DNS server agregara a las _queries_ que especifiquen solo el _hostname_ sin especificar el nombre completo del dominio. 

Para verificar las asignaciones realizadas se puede usar `show hosts`

![[Pasted image 20250116075451.png]]



