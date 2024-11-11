---
tags:
  - VLAN
  - CCNA
---
_Dynamic Trunking Protocol (DTP)_, es un protocolo propietario de Cisco que permite a los switches Cisco determinar automaticamente el _operational mode_ de sus puertos, este puede ser access o trunk.
- Un puerto configurado manualmente en modo access (administrative mode) va a operar siempre como un access port (operational mode)
- Un puerto configurado en manualmente en modo trunk (administrative mode) va a operar siempre como un trunk port (operational mode)

Al usar DTP, los switches vecinos intercambian mensajes DTP informandose entre si su _administrative mode_ configurado en el puerto (que se configuran con `switchport mode <mode>`). Los cuales deciden el _operational mode_ apropiado dependiendo de la combinación que haya en la configuración de los administrative mode.

## DTP negotiation
Además de configurar los puertos manualmente con `trunk` o `access`, tambien se puede hace uso de `switchport mode dynamic auto` y `switchport mode dynamic disarable`.

![[Pasted image 20241110013023.png]]
En la imagen anterior los dos switches en la interface G0/0 tenian `dynamic auto` como modo administrativo. El resultado de esto es que se forma un access link. 

> `dynamic auto` es el default administrative mode en todos los puertos de un [[switch]] Cisco. 

Para ver los _operational mode_ y _administrative mode_ de un puerto se usa el comando `show interface <interface> switchport`. 

![[Pasted image 20241110024213.png]]
Para formar un trunk link entre dos [[switch]]es podemos establecer uno de los puertos como `dynamic disarable`. 

![[Pasted image 20241110024439.png]]

**Swich port administrative modes**

![[Pasted image 20241110024525.png]]

> Aunque se configure un trunk port manualmente, este aun envia mensajes DTP. Esto para asegurar que el otro extremo tambien pueda operar como un trunk port mode (si el vecino es `dynamic auto` o `dynamic disarable`)

**Resultados en _operational mode_ para cada combinación de _administrative mode_**  
![[Pasted image 20241110024913.png]]

## Disabling DTP 
Por motivos de seguridad y para reducir el ruido en el trafico, se recomienda deshabilitar DTP. Ya que esta es vulnerable a que un dispositivo externo pueda formar trunk ports (al tener todos los puertos en modo `dynamic auto` por defecto) enviando mensajes DTP. 
![[Pasted image 20241110025840.png|500]]

Para deshabilitar DTP se puede configurar manualmente la interface con `switchport mode access` o bien se puede hacer uso del comando `switchport nonegotiate` el cual se recomienda para asegurar que no envíen mensajes DTP. 