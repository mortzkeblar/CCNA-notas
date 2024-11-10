Este concepto hace referencia al enrutamiento entre subredes en una LAN que esta segmentada mediante [[VLAN]]s. 

El metedo más simple (no recomendado) es el uso de un enlace por cada [[VLAN]] creada, esto tiene el problema de que por la cantidad de VLANs que podemos crear es proporcional a la cantidad de puertos disponibles.

![[Pasted image 20241109192810.png|500]]

Esto es una limitación si la cantidad de VLANs que necesitamos es mayor a la de puertos disponibles.

## Router on a stick 
_Router on a stick (ROAS)_, este metodo implicar el uso de un unico puerto físico configurado como un enlace trunk o [[IEEE 802.1Q]] entre un [[switch]] y un router. De lado del router se deben crear subinterfaces, cada uno de estas con su propia IP address. 

![[Pasted image 20241109193123.png|500]]

> La interface fisica de un router y sus subinterfaces comparten la misma MAC address. Cuando un frame llega a un interface fisica, el router reenvia el frame a la subinterface en base al VLAN tag (ver [[IEEE 802.1Q]]) que se encuentra dentro del frame en vez de la MAC address de destino. 


``` bash
# SW1 configuration 
interface g0/1 
switchport trunk encapsulation dot1q 
switchport mode trunk 
switchport trunk allowed vlan 10,20,30
switchport trunk native vlan 999

# R1 configuration for VLAN 10, is same for others VLANs 
interface g0/0 
no shutdown 
interface g0/0.10    # It does not need to match the VLAN, although it should.
encapsulation dot1q 10  # Must match the configured VLAN 
ip address 172.16.1.1 255.255.255.192

```

### Native VLAN in ROAS 
Existen dos metodos para configurar una [[VLAN Native]] en una modelo ROAS.
- Usar el comando `encapsulation dot1q <vlan-id> native` dentro de la subinterface a configurar
``` bash
interface g0/0
no shutdown 
interface g0/0.10
encapsulation dot1q 10 native 
ip address 172.16.1.1 255.255.255.192 
```

- Configurar una IP address para la VLAN native sobre la interface fisica (no sobre una subinterface)

``` bash
interface g0/0
no shutdown 
ip address 172.16.1.1 255.255.255.192 
```

## Multilayer switching 

Este metodo hace uso de [[switch]]es multilayer los cuales poseen un router integrado, estos permiten crear interfaces virtuales llamadas _Switch Virtual Interfaces (SVIs)_, cada SVI es un interface asociada con un [[VLAN]] y una IP address que los hosts finales usan como [[default gateway]]. 

![[Pasted image 20241109214514.png|500]]

En un [[switch]] multilayer, es necesario habilitar IP routing con el comando `ip routing`. Sin esto, el switch no puede hacer forwarding de paquetes entre subredes/VLANs. 

``` bash
# SW1 config 
ip routing 
interface vlan 10 
ip address 172.16.1.1 255.255.255.192 
interface vlan 20
ip address 172.168.1.65 255.255.255.192 
interface vlan 30
ip address 172.16.1.129 255.255.255.192 
```

Este al igual que un router, posee una [[routing table]] en la que se agregan [[local route]] y [[connected route]]s. Por lo cual no es necesario agregar static routes de forma manual.

![[Pasted image 20241109235619.png|500]]

Para que una SVI pueda funcionar, necesita cumplir una serie de requisitos. El estado se puede comprobar con `show ip interface brief`.
- La [[VLAN]] asociada al SVI debe estar creada en el switch (con `vlan <vlan-id>`)
- El switch debe cumplir al menos alguno de los requisitos 
	- Un access port asociado con la VLAN (con estado UP/UP)
	- Un trunk port en la que ese VLAN este permitida (con estado UP/UP)
- La VLAN debe estar habilitada (no debe tener aplicado `shutdown`)
- El SVI debe estar habilitado (no debe tener aplicado `shutdown`)

![[Pasted image 20241109235640.png]]