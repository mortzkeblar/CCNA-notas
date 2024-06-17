---
tags:
  - ACL
  - CCNA
---

#### Example 1
_Que hace esta ACL?_
![](_anexos_/Screenshot%20from%202023-12-29%2000-49-18.png)
``` 
- Permita el acceso de la IP 10.0.0.1 a toda la red 20.0.0.0/24
- Denega el acceso de todo el rango IP <10.0.0.0 - 10.0.0.255> hacia cualquier IP dentro del rago <20.0.0.0 - 20.0.0.255>
- Permite el acceso de paquetes que vengan de la IP 10.0.0.2 a través de TCP a la IP 200.3.4.5 mientras el puerto de destino sea igual a 80
```

#### Example 2
![](_anexos_/Screenshot%20from%202023-12-29%2000-57-31.png)
``` bash 
R1(config)$ access-list 100 deny tcp host 199.34.209.1 host 192.168.2.2 eq 22
R1(config)$ access-list 100 permit ip any any # IMPORTANTE para no bloquear otras IPs
R1(config)$ interface Fa0/1
R1(config)$ ip access-group 100 in
```

# Exercices

### Exercise 1
![](_anexos_/Screenshot%20from%202023-12-29%2001-23-51.png)
``` bash

## Solución (MAL hecha), esto no es una solución
R1(config)$ access-list 101 deny tcp host 192.168.0.50 any eq 80
R1(config)$ access-list 101 deny icmp any host 192.168.0.0 0.0.0.255
R1(config)$ interface fa0/0
R1(config)$ ip access-group 101 out

R1(config)$ access-list 100 permit 192.168.0.0 0.0.0.255 any
R1(config)$interface fa0/1
R1(config)$ip access-group 100 in
```

_Solución_
![](_anexos_/Screenshot%20from%202023-12-29%2001-55-01.png)
``` bash
# La misma solución de la imagen pero codificada
R1(config)$ ip access-list extended TRAFICO_HACIA_INTERNET
R1(config-ext-nacl)$ deny tcp host 192.168.0.50 any eq 80
R1(config-ext-nacl)$ permit ip any any
R1(config)$ interface fa0/0
R1(config-if)$ ip access-group TRAFICO_HACIA_INTERNET in

R1(config)$ ip access-list extended TRAFICO_DESDE_INTERNET
R1(config-ext-nacl)$ deny icmp any any echo
R1(config-ext-nacl)$ permit ip any any
R1(config)$ interface f0/1
R1(config)$ ip access-group TRAFICO_DESDE_INTERNET in
```

> En una situación de bloqueo es mejor aplicar el flltro en la interfaz de entrada y no de salida, porque eso implica ser procesado por el router (consumiendo recursos en el paso) solo para que al final salga filtrado. 

### Exercise 2
![](_anexos_/Screenshot%20from%202023-12-29%2002-13-49.png)
``` bash
R1(config)$ ip access-list extended VTY_RESTRICTED
R1(config-ext-nacl)$ permit 192.168.0.128

R1(config)$ line vty 0 4
R1(config-line)$ access-class VTY_RESTRICTED in

## El motivo de esa configuración es porque si restringimos acceso a una IP admin, implica configurar la restricción en cada interfaz del router. 
```


