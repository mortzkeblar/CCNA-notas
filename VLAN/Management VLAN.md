---
tags:
  - configuration
  - example
  - VLAN
---

Se trata de crear una VLAN administrativa para que se pueda gestionar de manera remota a través de SSH o Telnet. 
Tambien puede ser util a la hora de hacer troubleshooting. `Por ejemplo` cuando existen problemas de conectividad entre el router y el host, el cual involucra un switch de por medio. 

``` bash
IOU2(config)$ interface vlan 10
IOU2(config-if)$ do show ip interface brief
...
Vlan10   unassingned   YES  unset  administratively down down
...

IOU2(config-if)$ no shutdown
IOU2(config-if)$ip address 192.168.10.2 255.255.255.0

```

> NOTA: un error frecuente al configurar un host es usar la IP de switch (VLAN administrativa) como puerta de enlace.
> El switch al ser un dispositivo de capa 2. Es transparente al proceso que pueda realizar el protocolo IP. 

Si tratamos de realizar `ping` desde el switch a una IP que no este en el rango de su red, este no va a responder. Por ejemplo de IOU2 `192.168.10.2` -> R1 `192.168.20.1`. Esto no pasa con la R1 `192.168.10.1`.
Para solucionar eso:
``` bash
IOU2(config)$ ip default-gateway 192.168.10.1
```

Se pueden generar IPs para cada VLAN que pase por el switch. Pero solo se puede agregar una  puerta de enlace. 