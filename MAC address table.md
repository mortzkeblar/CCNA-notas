La tabla CAM (Content Addressable Memory), asocia direcciones MAC de dispositivos conectados con los puertos específicos del [[switch]].

Las direcciones MAC las obtiene a medida que se va enviando trafico entre dispositivos, si un dispositivo no se encuentra en la tabla CAM, el switch envía mensajes broadcast a los dispositivos que se encuentran conectados, si el [[switch]] recibe una respuesta, este agregara la MAC address y el puerto de origen a la tabla CAM.

> Se debe considerar que la tabla CAM solo almacena direcciones de origen, mas no de destino. Por eso nunca va a ver un broadcast MAC address en la tabla. 

![[_anexos_/Pasted image 20240518234932.png|500]]

El almacenamiento de direcciones es por tiempo limitado, si no se escucha trafico desde ese puerto durante un periodo de tiempo establecido, la entrada se elimina de la tabla. 

``` 
Switch#show mac address-table aging-time

Global Aging Time:  300

Vlan    Aging Time

—-    ———

Switch#conf t

Switch(config)#mac-address-table aging-time 600
```

