---
tags:
  - CCNA
date created: Saturday, October 19th 2024, 11:08:14 pm
date modified: Wednesday, November 20th 2024, 11:33:50 pm
---
La tabla CAM (Content Addressable Memory), asocia direcciones MAC de dispositivos conectados con los puertos específicos del [[switch]].

Las direcciones MAC las obtiene a medida que se va enviando trafico entre dispositivos, si un dispositivo no se encuentra en la tabla CAM, el switch envía mensajes broadcast a los dispositivos que se encuentran conectados, si el [[switch]] recibe una respuesta, este agregara la MAC address y el puerto de origen a la tabla CAM.

> Se debe considerar que la tabla CAM se construye a partir de MAC address que se encuentran en el campo de origen dentro de un frame, mas no de destino. Por eso nunca va a ver un broadcast MAC address en la tabla. 

![[_anexos_/Pasted image 20240518234932.png|400]]

El almacenamiento de direcciones es por tiempo limitado, si no se escucha trafico desde ese puerto durante un periodo de tiempo establecido, la entrada se elimina de la tabla. 

``` 
Switch#show mac address-table aging-time
Global Aging Time:  300
Vlan    Aging Time
—-    ———
Switch#conf t
Switch(config)#mac-address-table aging-time 600
```

## MAC address table in Cisco IOS 
Para ver la MAC address table en un switch Cisco se usa el comando `show mac address-table`. 

![[Pasted image 20241023032538.png|500]]
> Las MAC address de los switches no desempeñan funciones en la forwarding de mensajes entre hosts, pero si se utilizan para el envio de mensajes entre los propios switches. 

``` bash
# borrar los datos de la MAC address table manualmente 
clear mac address-table dynamic 

# borrar una MAC address especifica 
clear mac address-table dynamic <ADDRESS> 

# borrar todas las MAC address de una interface 
clear mac address-table dynamic <INTERFACE> 
```