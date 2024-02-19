https://www.youtube.com/watch?v=1RpvaxKwQTA&list=PLoqM5eIpDpUEhtUNKuOd-sGll7NrvlwD7&index=14
Se trata de una restricción de puertos a nivel de MAC address.
- Es decir, cuando un dispositivo se conecte al switch. Este va a tomar su MAC address y lo va a asociar a una interfaz. 
- Si se conecta un dispositivo que no esta asociado a un puerto, el switch puede restringir o apagar el acceso por esa interfaz

> Nunca se deben configurar la seguridad de los puertos en las interfaces troncales


``` bash
SW(config)$ int range f0/1-24

## Permitimos que el SW capture las MAC de forma dinamica, sticky
SW(config-if-range)$ switchport port-security mac-address sticky
SW(config-if-range)$ switchport port-security maximun 1
SW(config-if-range)$ switchport port-security violation protect  # other options: restrict, protect, shutdown
SW(config-if-range)$ switchport port-security
```
Se denomina _puerto dinamico_ porque ese puerto no esta asociado a una VLAN.

``` bash
SW(config)$ show port-security interface f0/1   
SW(config)$ show port-security address
```

``` bash
# Configuracion estatica?
SW(config)$ int f0/1
SW(config-if)$ switchport mode access
SW(config-if)$ switchport access vlan 1 # esto no es necesario
SW(config-if)$ switchport port-security   
```