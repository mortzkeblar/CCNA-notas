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






