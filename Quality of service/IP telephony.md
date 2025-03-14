Tambien llamada **_Voice-over-IP (VoIP)_**, esta nota hace referencia a la comunicación de telefonos usando el protocolo IP. 

> _VoIP_ reemplaza a las viejas conexiones PSTN (public switched telephone network), tambien conocida como POTS

El tráfico VoIP es particularmente importante en [[QoS]] ya que suele ser muy sensible cuando la red tiene una degradación, por lo que es importante priorizar ese tráfico mediante politicas de [[QoS]]. 

Los _IP Phones_ integran un pequeño _three-port switch_, lo que permite que tanto el telefono como la PC necesiten un solo cable UTP para poder conectarse a un dispositivo de red como un [[switch]]. 

![[Pasted image 20250312230318.png]]

## Voice [[VLAN]] 
En la imagen anterior se observa que tanto la PC como el telefono, comparten la misma switch port para poder obtener conectividad. 

Debido a que el llamadas de voz son especialmente sensibles a la latencia y/o perdidas de paquetes, es necesario segmentar el trafico de estos dispositivos mediante politicas de [[QoS]] para poder priorizar el trafico de voz por encima del _data traffic_. 

La segmentación se realiza mediante [[VLAN]]s, en este caso se usa una caracteristica especial llamada **_Voice VLAN_**. 

![[Pasted image 20250313010230.png]]
Este modo permite que una interface sea configura en modo `access` y aun así se pueda traficar dos [[VLAN]]s sobre el enlace. En este caso se diferencian dos tipos de VLAN:
- _Data VLAN_ (tambien llamada _access VLAN_) - es por donde va el trafico de datos y es **untagged**
- _Voice VLAN_ - es por donde va el trafico de voz, este opera de forma **tagged**, y se configurado usando `switchport voice vlan <vlan>`

![[Pasted image 20250313012011.png]]

## Power over Ethernet (PoE)
**Power over Ethernet (PoE)** es una tecnologia que permite alimentar electricamente a un telefono mediante el mismo cable ethernet que se usa para la transmisión de datos, por lo que no es necesario un cable de alimentación adicional. 

> PoE no es una caracteristica exclusiva de los telefonos, sino que se usan en varios dispositvos como: IP camaras, access points, door locks, etc.

![[Pasted image 20250313012747.png]]
En la imagen se observa como el PSE (Power Sourcing Equipment) de SW1 proporciona energia eléctrica a los IP phones - PDs (Powered Devices) mediante PoE. 

El PSE realiza una serie de verificación antes de enviar suministro eléctrico mediante el cable ethernet, debido a que existe el riesgo de dañar el equipo si este no soporta PoE o bien se le envia demasiada energia: 
- _Detection_ - cuando se conecta un dispositivo, el PSE realiza un check inicial aplicando un bajo voltaje sobre el cable para verificar si el dispositivo soporta PoE 
- _Classification_ - el PSE evalua la cantidad de energia que necesita el PD, y asigna esa cantidad de energia de su suministro disponible
- _Startup_ - el PSE suministra al PD del voltaje necesario para iniciar el dispositivo 
- _Normal operation_ - el PSE suministra energia al PD y monitorea que este se entregue de forma segura y constante 

En un [[switch]] con PoE-enabled, se puede verificar el estado de los PDs conectados mediante `show power inline`

![[Pasted image 20250313014247.png]]
El campo _class_ indica la necesidad de suministro electrico que necesita un dispositivo, cuanto mayor sea el número indica que se necesita suministrar más energia. 

Las configuraciones de PoE puede ser modificadas mediante el comando `power inline` en el _interface config_ mode, la cual presenta algunas opciones: 
- `power inline auto` - comando aplicado por defecto, en este modo es el [[switch]] el que asigna de forma automatica la cantidad de energia que necesita un dispositivo después de detectar que soporta PoE 
- `power inline state [max <milliwatts>]` - comando usado para asignar energia manualmente a un puerto, esta es reservada incluso cuando no hay ningun PD conectado en el puerto. 
	- Si no se especifica el valor del comando, se asigna el valor maximo de asignación
		- Cuando un dispositivo se conecta, el [[switch]] espera a detectar el PD y así poder analizar las necesidades energeticas de este para suministrar la cantidad de energia apropiada para el dispositvo. 
			- En caso de que se asigne manualmente mayor energía de la necesaria, la cantidad especificada queda reservada a ese puerto a pesar de que no se este usando.
- `power inline never` - desactiva PoE en el puerto 

> ##### Layer 2 Discovery Protocols in PoE 
Los _[[Layer 2]] discovery protocols_ como [[CDP]] / [[LLDP]] pueden ser usadas para permitir a los dispositivos comunicar sus requerimientos de PoE, esta negociación ayuda al [[switch]] para asegurarse de entregar la energia necesaria así como para detectar en caso de que un dispositivo pueda estar extrayendo demasida energia del [[switch]]

Si un PD extrae demasida energia del [[switch]], este ultimo puede ser dañado. Por lo que se tiene un caracteristica llamada _power policing_:
- Cuando un PD comienza a extraer demasida energia (más alla del _cutoff_), el switch va poner ese puerto en un estado _error-disabled_, en la que detiene la transmisión de datos y energia 
	- Para rehabilitar el puerto _error-disabled_, se puede hacer `shutdown`
 y `no shutdown`, es importante desconectar primero el dispositivo problematico 

Esta función se habilita mediante `power inline police` en el _interface config_ mode. 

![[Pasted image 20250314005021.png]]


