> Las ACL Standard filtran paquetes IP basándose unica y exlusivamente en la _dirección IP de origen_. Son las ACL más antiguas y las que raramente se utilizan para filtrar tráfico.

![](_anexos_/Screenshot%20from%202023-12-28%2011-41-24.png)

_Que hace exactamente ACL1?_
``` bash
Router(config)$ access-list 1 permit host 192.168.60.1
Router(config)$ access-list 1 permit deny 192.168.60.0 0.0.0.3
Router(config)$ access-list 1 any
```

- Permite el acceso a `192.168.60.1`
- Bloque el acceso  entre las direcciones `192.168.60.0` y `192.168.60.3`, pero respeta la orden anterior
- Permite el acceso a cualquier IP

_Tambien: [ACL Standard - CLI Command](ACL%20Standard%20-%20CLI%20Command.md)_


### Rules ACL Standard

> Algo importante a recordar es que las declaraciones más especificas (i.e. `access-list 101 deny tcp host 10.0.3.2 host 192.168.5.10`) deben colocarse antes que las declaraciones más genericas (i.e `access-list 101 deny tcp any any`).
> Esto porque IOS deja de evaluar las ACL una vez que se encuentra la primera declaración que coincide. 

_Ver: [ACL Standard - List of rules and laws (IMPORTANT)](ACL%20Standard%20-%20List%20of%20rules%20and%20laws%20(IMPORTANT).md)_

#### Exercise

![](_anexos_/Screenshot%20from%202023-12-28%2013-03-09.png)

``` bash
# Solución 1
access-list 1 deny host 200.23.4.5
access-list 1 permit 200.23.4.0 0.0.0.7
access-list 1 permit 200.23.4.8 0.0.0.1
access-list 1 permit 200.23.4.10 0.0.0.0

# Solución 2
access-list 2 permit host 172.30.244.17
access-list 2 permit host 172.30.244.18
access-list 2 permit host 172.30.244.20
access-list 2 permit 172.30.244.250 0.0.0.1
access-list 2 permit 172.30.244.252 0.0.0.1
access-list 2 permit host 172.30.244.254
```