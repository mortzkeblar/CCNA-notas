``` bash
# Comandos relevantes para depuración 
SW1$ show vlan brief

SW1$ show interfaces trunk
```

La idea de este lab es implementar el protocolo _VTP_ para que las VLAN se configuren de forma dinamica en los switches.

![](_anexos_/Screenshot%20from%202024-01-10%2004-30-49.png)

Primero habilitamos el servidor VTP en el switch central, este es el que va a actuar como VTP server.

``` bash
# Creamos las VLANs 
SW1$ conf t
SW1(config)$ vlan 100
SW1(config-vlan)$ vlan 101
SW1(config-vlan)$ vlan 102
SW1(config-vlan)$ vlan 103
SW1(config)$ exit

# Configuramos el servidor VTP
SW1(config)$ vtp mode server
SW1(config)$ vtp domain cisco.com 

# Configuramos el trunk en las cuatro interfaces 
SW1(config)$ interface range g0/0-3
SW1(config-if-range)$ switchport trunk encapsulation dot1q 
SW1(config-if-range)$ switchport mode trunk 
SW1(config-if-range)$ switchport trunk allowed vlan 100,101,102,103

# Configuramos la VLAN nativa en 1 (default)
SW1(config-if-range)$ switchport trunk native vlan 1
```

- No necesitamos configurar los otros switches ya que VTP se encarga de la negociación
	- Lo que si es que debemos habilitar eso
- Recordar que los _frames_ que viajan por VLAN nativa son todos aquellos que no tienen una etiqueta VLAN
- Importante cuando se configure el dominio con `vpt domain domain.example` entre los switches, debe ser igual en todos los dispositivos. 
- En los dispositivos L3 es requisito pasar por comando `switchport trunk encapsulation dot1q`

Ahora habilitamos VTP en el cliente y la configuración para el host
``` bash
SW2> ena
SW2$ conf t
SW2(config)$ vtp mode client
SW2(config)$ vtp domain cisco.com
SW2(config)$ int g0/1
SW2(config-if)$ switchport mode access
SW2(config-if)$ switchport access vlan 100
SW2(config-if)$ exit

# Replicar la misma configuración para los otros tres switches con sus respectivas VLANs
```

Por ultimo, realizamos el enrutamiento en el switch MLS principal para poder conectar los hosts de las distintas VLANs.
``` bash
# enrutamiento  entre VLANs desde el switch principal
SW1(config)$ interface vlan 100
SW1(config-if)$ ip address 192.168.100.1 255.255.255.0
SW1(config-if)$ description VLAN100
SW1(config-if)$ no shut
SW1(config-if)$ exit

# Realizar los mismos pasos para las demas VLANs (en este mismo switch, claro)
```

Y por ultimo habilitamos el enrutamiento en el switch MLS con 
``` bash
SW1$ ip routing
```