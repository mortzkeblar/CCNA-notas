**Dynamic Host Configuration Protocol (DHCP)** es un protocolo que permite automatizar la asignación de [[IP address]], [[Default gateway]] y otras configuraciones de red que se le asigna a un host, definimos _host_ como cualquier dispositivo que envia y recibe paquetes sobre la red. 

> DHCP usa [[UDP]] como protocolo de [[Layer 4]]
> - DHCP server usa UDP port 67
> - DHCP cliente usa UDP port 68 

DHCP usa un modelo cliente - servidor en donde los clientes envia request al DHCP server, este ultimo realiza el _lease_ o alquiler de direcciones IP a los hosts y proporciona otras configuraciones a los clientes como el [[Default gateway]]. 

> DHCP es _stateful_, que significa que realiza el seguienmiento de las direcciones que _leases_ a los clientes. Se contrasta con el modelo [[NDP#SLAAC]] de [[IPv6]]. 

![[Pasted image 20250117024254.png]]

## Leasing an IP address with DHCP 
Para que un cliente pueda alquilar una [[IP address]] de un DHCP server, siguen el siguiente proceso.
- _DISCOVER_ - cliente busca servidores DHCP y les indica que necesita una IP address 
- _OFFER_ - server ofrece una IP address al cliente (junto a otras confs)
- _REQUEST_ - cliente acepta la IP address ofrecida 
- _ACK (acknowledgment)_ - server confirma y finaliza el _lease_

![[Pasted image 20250117024950.png]]
Este proceso de arrendamiento, esta tambien conocido como _DORA_. 

- El _DISCOVER_ message es enviado por un cliente a la dirección broadcast 255.255.255.255, ya que desconoce la dirección del DHCP server (si es que existe en la red)
	- Como el cliente aún no tiene una IP asignada, la IP de origen en el paquete es la dirección reservada `0.0.0.0`
- Una vez que el DHCP server reciba el _DISCOVER_ message, este le responde con un _OFFER_ message, donde le ofrece una IP al cliente. 
	- El _OFFER_ message enviado es de tipo unicast y suele tener como destino la IP que se le ofrece, pero como el host aún no tiene una IP asignada este no puede recibir el _OFFER_. Para solvertar esto, el cliente lo puede aclarar en el _DISCOVER_ message inicial, para que el DHCP server envie el _OFFER_ message dirigido a la dirección Broadcast 255.255.255.255. 
- Una vez que el cliente recibe el _OFFER_, este lo acepta enviando un _REQUEST_ message. 
	- La IP de origen del _REQUEST_ enviado sigue siendo 0.0.0.0 y el destino tambien se mantiene como la dirección broadcast 255.255.255.255, a pesar de que el cliente ya conoce su IP arrendado así como la del DHCP server por el _OFFER_ recibido
		- Esto ocurre para cubrir casos de multiples servidores DHCP dentro de la red, ya que el cliente acepto una oferta especifica. El broadcast avisa a los demas servidores que su oferta fue rechazada, por lo que liberan la IP ofrecida.  
- Una vez confirmada y finalizada el _lease_, el server envia un _ACK_ message. Al igual que el _OFFER_ este puede enviado de forma unicast o broadcast, dependiendo de las capacidades del cliente. 

### Lease renewal 
El tiempo de arrendamiento o _lease_ no es permanente, puede variar dependiendo la configuración establecida. El cliente tiene la posibilidad de renovar el contrato de arrendamiento para mantener la IP, para esto envia un _REQUEST_ message al DHCP server (de tipo unicast).

Generalmente el cliente envia el REQUEST cuando ya paso el 50% del tiempo establecido del _lease_. Una vez recibido el _REQUEST_, el server responde con un _ACK_ message confirmando la renovación del lease. 

## Cisco IOS as a DHCP server 

![[Pasted image 20250117040654.png]]

#### Configuring a DHCP pool 

![[Pasted image 20250118020921.png]]
- Si anteriormente ya se configuro la exclusión de IPs que no deben ser _lease_ con `ip dhcp excluded-address <low-ip> <high-ip>`, cuando se use `network` comando con un subred completa, esta va a respetar las direcciones excluidas
- `domain-name <domain>` es un parametro opcional que permite especificar a los clientes el dominio por defecto, esto facilita a la hora de comunicarse con los hosts ya que no necesitamos especificar el FQDN 
- `lease <days> <hours> <minutes>`  es un parametro opcional permite especificar el tiempo de arrendamiento, el valor por defecto es de 24 horas
	- Tambien se puede usar `lease infinity` para que el tiempo de arrendamiento no tenga limite

Para verificar que clientes con los que se hizo _lease_, se puede usar `show ip dhcp binding`

![[Pasted image 20250118021942.png]]
- Para limpiar las direcciones en el server _binding table_ se usa `clear ip dhcp binding { * | <ip-address> }`
- _Client-ID / Hardware address / User name_ representa a identificadores unicos que se utilizar para identificar a los clientes. Este idenficador generalmente se envia en el _DISCOVER_ message. 

#### Address conflicts 
Un problema que puede surgir es cuando un DHCP server quiere asignar una [[IP address]] que ya este en posesión de un host (i.e. configuración manual), en este caso se puede excluir esa dirección con `ip dhcp excluded-address`

![[Pasted image 20250118131132.png|400]]
Los Cisco IOS DHCP servers tienen la capacidad de detectar si una [[IP address]] ya esta siendo usada mediante el envió de pings. 

Antes de que el DHCP server puede enviar el _OFFER_ message en el proceso DORA, este enviara un ping a la IP que pretende arrendar. 
- En caso de que este responda, quiere decir que otro host ya la usando por lo que lo marca como _conflit_ y lo elimina del DHCP pool.
- Luego trata de asignar otra dirección y sigue el mismo proceso 

![[Pasted image 20250118134102.png]]
Para ver las direcciones conflictivas se puede usar `show ip dhcp conflict`, para borrar estas entradas ya sea que se solucionar esos conflictos, se puede usar `cliear ip dhcp conflict { * | <ip-address> }`


## CIsco IOS as a DHCP client 
Si bien los dispositivos de la infraestrucutura de red normalmente se le asigna direcciones estáticas, un uso comun de DHCP es cuando se utiliza como cliente para una conexión con un ISP. 

![[Pasted image 20250118134641.png]]

Para usar DHCP como cliente, solo basta con usar `ip address dhcp` en la _interface config mode_

![[Pasted image 20250118134729.png]]

## DHCP relay 
_DHCP relay_ es un termino que surge cuando se tiene una situación donde el servidor DHCP se encuentra en una ubicación remota o fuera de la [[LAN]] del cliente. En el proceso _DORA_ esto es un problema ya que el _DISCOVER_ y _REQUEST_ message se envian de forma multicast, este tipo de mensajes no se propagan más alla de la red donde se origino. 

Para esto, se configura un router que actua como _DHCP relay agent_ y reenvia los _DISCOVER / REQUEST_ broadcast messages como paquetes _unicast_ al servidor remoto.

![[Pasted image 20250118143811.png]]
Para configurarlo se usa el comando `ip helper-address <server-ip-address>` dentro de la configuración de la interface donde esta conectada el cliente, para que esta pueda interceptar los mensajes broadcast. Esta conf se puede verificar en la interface con `show ip interface <interface>`

![[Pasted image 20250118145459.png]]