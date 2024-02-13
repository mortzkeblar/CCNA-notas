---
tags:
  - general
  
---

##### Algunos detalles
- El comando `show` no se ejecuta dentro de una configuración global, es decir, cuando el _prompt_ sea del estilo `Router (config)#`
### General

``` bash
# Mostrar configuración en ejecución
Router$ show running-config
```

``` bash
# Cambiar el hostname de un router#
Router(config)$ hostname <nombre>
```

``` bash
# Guardar los cambios (importante)
Router$ copy running-config startup-config

# Tambien esta la opción de guardarlo con
Router$ write memory

## No existen diferencias entre los dos comandos más alla de que 'copy' le permite elegir el nombre de la copia, es decir, puede guardar varias configuraciones sin que se sobre escriban encima
```

``` bash
# Habilitar el servicio de cifrado de contraseñas, asi no muestra en texto plano al hacer running-config
Router(config)$ service password-encryption
```

#### Asignar una IP estática
``` bash
# IPv4
Router> enable
Router$
Router$ configure terminal
Router(config)$ interface <interface>      # G0/0, f0/0, fastEthernet 0/0, etc
Router(config-if)$ ip address <IP> <IP_mask>
Router(config)-if$ no shutdown             # paso IMPORTANTE en routers
```

``` bash
# IPv6
Router$ conf t
Router(config)$ interface <interface>
Router(config-if)$ ipv6 address 2000:1:1:AA::/64 eui-64  # IPv6 local
Router(config-ip)$ no shutdown

## Para asignar una IPv6 global (más alla de la red LAN) asignar:
Router(config)$ ipv6 address 
```

#### Route

``` bash
# Asignar una ruta de forma estática
Router(config)$ ip route <IP_ADDR> <SUBNET_MASK> <GATEWAY> 


## IP_ADDR: es la IP de la red a la que desea llegar, por ejemplo 192.168.0.0, 10.177.20.0
## SUBNET_MASK: por ejemplo para /24 255.255.255.0, para /30 255.255.255.252
## GATEWAY: hace referencia al 'next hop', la puerta de enlace para poder acceder a más redes. Generalmente siempre se ocupa la IP del 'siguiente salto' al configurar varias rutas estaticas conectadas entre sí. Por ejemplo:

Router(config)$ip route 192.168.20.0 255.255.255.0 10.177.0.1
Router(config)$ip route 10.177.0.8 255.255.255.252 10.177.0.1
Router(config)$ip route 10.177.0.8 255.255.255.0 10.177.0.1

## Como ven, el GATEWAY se mantiene.

## Para poder ver las rutas asignadas en el router ejecutar
Router$ show ip route
...
S        10.177.0.4/30 [1/0] via 10.177.0.1
S        10.177.0.8/30 [1/0] via 10.177.0.1
      192.168.10.0/24 is variably subnetted, 2 subnets, 2 masks
C        192.168.10.0/24 is directly connected, GigabitEthernet0/0
L        192.168.10.1/32 is directly connected, GigabitEthernet0/0
S     192.168.20.0/24 [1/0] via 10.177.0.1
S     192.168.30.0/24 [1/0] via 10.177.0.1
...

### S: static

```


``` bash
# Asignar una ruta por default (Default route)
Router(config)$ ip route 0.0.0.0 0.0.0.0 <IP_GATEWAY>

# Asignar un Default Route con la interfaz local de salida como parametro, por ejemplo 'fastEthernet 0/0' o 'G0/1'
Router(config)$ ip route 0.0.0.0 0.0.0.0 <INTERFACE>

## NOTA: siempre es preferible asignar la ruta por default a través de la dirección IP y no de la interfaz. Esto porque al ser asignado por IP, la maquina va a estar mandando constantemente mensajes ICMP verificando que la conexión este activa y tomar medidas en caso contrario.
## Por otra parte, al ser asignado por la interfaz, va a verificar que la interfaz local este activa a nivel fisíco. Pero no de lo que pase con la otra interfaz remota.  

## Esto esta relacionada con la advertencia:
##%Default route without gateway, if not a point-to-point interface, may impact performance

```

#### Banner
``` bash
Router(config)$ banner motd <any_character>
...
##### EL ACCESSO A ESTE DISPOSITIVO ESTA PROHIBIDO #####
##### SOLO USUARIOS PERMITIDOS ######
...

Router(config)$ line console 0
Router(config)$ motd-banner
Router(config)$ line vty 0 4
Router(config)$ motd-banner

## Curiosidad: buscar en google 'cisco ascii generator'

```
### Acceso Local 
``` bash
# Proteger el modo EXEC priv con una contraseña, tenemos dos metodos

Router(config)$ enable password <password>
Router(config)$ enable secret <secret>

## La diferencia entre 'secret' y 'password' es que en 'password' no cifra la contraseña, la cual se puede ver en texto plano al ejecutar:

Router$ show running-config

## Si tenemos configuradados contraseña por ambos metodos, el router va a dar preferencia a la contraseña configurada con 'secret'
```


``` bash
# Proteger el acceso a la consola con una contraseña

Router(config)$ line console 0
Router(config)$ password <console_password>
Router(config)$ login
Router(config)$ exit


# Tambien es posible crear un usuario y contraseña
Router(config)$ username <user> secret <secret>
Router(config)$ line console 0

# A partir de aca los comandos sirven para eliminar la configuración anterior
Router(config)$ no password
Router(config)$ no login

# Esto indica al router que va a consultar la lista de usuarios dentro de la db local
Router(config)$ login local
```

### Acceso Remoto
#### Telnet (TCP/23)
``` bash
# Habilitar acceso vía Telnet al router con contraseña
## vyt: virtual terminals, desde el 0 hasta el 4

Router(config)$ line vty 0 4 
Router(config-line)$ password <telnet_password>
Router(config-line)$ login

## Este comando sirve para especificar telnet, se supone que no es necesario.
Router(config-line)$ transport input telnet
```

``` bash
# Habilitar acceso vía Telnet al router con usuario y contraseña

Router(config)$ username <user> password <pass>
Router(config)$ line vty 0 4
Router(config)$ login local
```

#### SSH (TCP/22)
- Si el modelo lleva en su nombre _K9_ quiere decir que soporta comunicación cryptografica, entonces le da la posibilidad de usar SSH (no es 100% garantizado).
``` bash
# Configurar acceso via SSH
Router$ show version
...
IOS K9
...

Router(config)$ hostname R1
R1(config)$ ip domain name <dominio.ej>
R1(config)$ crypto key generate rsa modulus 1024
R1(config)$ username <user> secret <secret>
R1(config)$ line vty 0 4
R1(config-line)$ transport input ssh
R1(config-line)$ login local
R1(config-line)$ exit
R1(config)$ ip ssh version 2
R1(config)$ ip ssh time-out 15
R1(config)$ ip authentication-retries 3

## Para borrar la clave rsa
R1(config)$ crypto key zoroize rsa
```


