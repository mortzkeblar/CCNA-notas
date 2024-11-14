---
tags:
  - VLAN
  - CCNA
---
_Router on a stick (ROAS)_, este metodo [[Inter-VLAN routing]]  hace uso de un unico puerto físico (o aggregate) configurado como un enlace trunk o [[IEEE 802.1Q]] entre un [[switch]] y un router. De lado del router se deben crear subinterfaces, cada uno de estas con su propia IP address. 

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
