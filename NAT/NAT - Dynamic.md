En el NAT dinámico, las IP internas de cada cliente de una LAN se asocian dinamicamente con cada IP externa (pública).
- Red interna LAN (`inside`)
- Red externa WAN (`outside`)
``` text
El rango de IPs publicas que tiene el ISP es 200.1.1.1/29.
- Quiere decir que tiene 8 IPs disponibles
- Restamos 2 IPs (gateway y broadcast) entonces quedan 6 IPs utilizables
- Asumimos que otra IP que ocupada para conectarse con otro router del ISP.
- Quedan 5 IPs, de las cuales una IP se asigna como dirección IP a la interfaz f0/1 del router
- Los restantes 4 IPs se configuran para la red interna con dynamic NAT
```
![](_anexos_/Screenshot%20from%202023-12-29%2012-27-30.png)
``` bash
# Configuración global
R(config)$ ip nat pool RANGO_PUBLICO 200.1.1.2 200.1.1.4 netmask 255.255.255.248
## Seleccionar los hosts permitidos para salir a internet
R(config)$ access-list 1 permit 192.168.0.0 0.0.0.255
## Asociar la red interna permitida con el pool de IPs publicas
R(config)$ ip nat inside source list 1 pool RANGO_PUBLICO
# Configuración interna
## Configuramos la interface para la red interna
R(config)$ interface fastEthernet0/0
R(config-if)$ ip net inside
R(config-if)$ exit
## Configuración para la red externa (hacia fuera de la LAN)
R(config)$ interface fastEthernet0/1
R(config-if)$ ip nat outside
```
- Cada IP interna cuando quiera salir a internet se conecta de forma dinamica a un IP publica asignada a internet. 
	- El problema es que necesitas `x` cantidad de IPs publicas como los hosts que quieran conectarse.
		- Quiere decir, si tienes 20 hosts que salen a internet desde la red interna. Necesitas 20 IPs pubilicas para que todos puedan estar conectadas de forma simultanea.
- En este red, el trafico de la LAN siempre va hacia internet. Pero no lo contrario, el trafico de internet  hacia la LAN no esta permitido.
- Es debido a esto que Dynamic NAT no se use en la industria.
> Acronimo: INIS -> `ip nat inside source`
