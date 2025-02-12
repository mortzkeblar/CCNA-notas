---
tags:
  - NAT
  - CCNA
date created: Saturday, October 19th 2024, 11:08:14 pm
date modified: Wednesday, November 20th 2024, 11:33:50 pm
---
#TODO agregar mas teoria
En el NAT estático es posible asignar estáticamente una dirección IP publica única a una dirección IP privada, asegurando disponibilidad y acceso desde internet hacia un servidor ubicado en la LAN.
- Tambien llamado `NAT 1:1`
- Este método puede convivir con otros mecanismos de NAT
- Uno de los tipos de NAT más usado actualmente
 
``` txt
En este caso se toma una IP publica asociada a la interfaz externa (diferente de la IP de la interfaz) y se asocia a un dispositivo interno de la red. En el ejemplo de abajo podemos ver que 200.1.1.5 esta asociada de forma manual con un servidor web con IP local 192.168.0.10/24 
```

 ![](Screenshot%20from%202023-12-31%2017-32-45.png)

``` bash
Router(config)$ ip nat inside source static 192.168.0.10 200.1.1.5
Router(config)$ interface f0/0
Router(config-if)$ ip nat inside
Router(config-if)$ exit
Router(config-if)$ interface f0/1
Router(config-if)$ ip nat outside
```

