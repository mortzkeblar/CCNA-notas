Una dirección IPv4 es un número de 32 bits que se representa en octetos de 8 bits y que idenfica un host de capa 3 dentro del modelo TCP/IP. 

> A diferencia de una MAC address (que tiene su destino como la dirección next-hop), una IP address envia su mensaje al destinatario final. 

![[Pasted image 20241025095436.png|400]]

## Prefix Length 
El tamaño de la porción de red puede ser indicado mediante el tamaño de prefijo (o _prefix length_) en el formato $/X$ donde X indica el número de bits que ocupa la porción de red. 

> La porción de red tambien es llamado comunmente _prefix_ o _network prefix_

- Todos los hosts que estan dentro de una misma [[LAN]] tienen la misma porción de red 
- Cada host que se encuentra dentro de la misma [[LAN]] tiene un unico porción de host

![[Pasted image 20241025124843.png|500]]

## Netmask 
Tambien llamado mascara de red. se trata de una cadena de 32 bits que se empareja con la IP address para indicar los bits que pertenecen a la porción de red y a la porción de host. 

![[Pasted image 20241026020511.png|500]]
> Una netmask siempre consiste en una serie de 1s seguido de una serie de 0s. Que indican la porción de red (los bits más significativos) y la porción de host (los bits menos significativos). Mascaras como 0.0.0.222 o 255.0.255.0 no son posibles. 

## Configuring IPv4 addresses on a Cisco router 

![[Pasted image 20241026020923.png|400]]


``` bash
enable 
configure terminal 
interface g0/0
ip address 192.168.1.1 255.255.255.0 
no shutdown
interface g0/1
ip address 192.168.2.1 255.255.255.0 
no shutdown 


# verification 
show ip interface brief 

# verification, if netmask is correct
show ip interface <interface-name>
```

![[Pasted image 20241026021339.png]]
- Status: lista el estado fisico de interface 
- Protocol: indica si el estado del protocolo de capa 2 de la interface esta funcionando

