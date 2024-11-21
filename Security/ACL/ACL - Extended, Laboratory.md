---
tags:
  - ACL
  - lab
  - CCNA
date created: Saturday, October 19th 2024, 11:08:14 pm
date modified: Wednesday, November 20th 2024, 11:33:50 pm
---

_Ver: [ACL - Extended, Examples](ACL%20-%20Extended,%20Examples.md)_
![](_anexos_/Screenshot%20from%202023-12-29%2008-36-59.png)

### GNS3 
![](_anexos_/Screenshot%20from%202023-12-29%2009-37-51.png)

#### Solución

_Ver: [ACL - Extended, CLI Command](ACL%20-%20Extended,%20CLI%20Command.md)_

``` bash
ip access-list extended SEC_RED10
## Punto 1
deny tcp host 10.0.0.1 host 30.0.0.1 eq 80
deny tcp host 10.0.0.1 host 30.0.0.1 eq 443
## Punto 2
deny icmp host 10.0.0.2 host 30.0.0.1 echo  # Todo: investigas mas sobre PING echo y reply
## Punto 4
deny ip 10.0.0.0 0.0.0.255 20.0.0.0 0.0.0.255
permit ip any any

## Aplicación en R1 (e0/0)
int e0/0
ip access-group SEC_RED10 in

```


``` bash
ip access-list extended SEC_WEB
## Punto 3
deny icmp host 30.0.0.1 any echo
permit ip any any

## Aplicación en R3 (e0/0)
int e0/0
ip access-group SEC_WEB in

```

> Importante: en el examen, por lo general suele estar un ejercicio relacionado a web (puerto 80 y 443) y algun otro protocolo conocido (como SMTP). Lo que si esta de manera fuerte es el relacionado con ICMP (ping).

``` bash
## Punto 5
access-list 1 permit host 10.0.0.1
line vty 0 4
access-class 1 in
### La siguiente instrucción niega implicitamente a todas las demas IPs

## Segun la indicación esto debe ser aplicado en los tres routers
```
Esta instrucción impide la conexión a los routers sin importar la interfaz que tenga (quiere decir, sin importa a que IP del router quiera conectarse).,