---
tags:
  - ACL
  - lab
  - CCNA
---

![](_anexos_/Screenshot%20from%202023-12-28%2023-12-58.png)

### Consigna
![](_anexos_/Screenshot%20from%202023-12-28%2023-13-32.png)

### Solución
> Es importante trazar una solución en 'papel' antes de aplicar en un router en producción. Una mala configuración puede implicar quedarnos desconectados del router (si estamos de forma remota) o peor aún, desconectar a toda la organización.

``` bash
## R3 (Consigna 1)
access-list 1 permit host 10.0.0.2
access-list 1 deny 10.0.0.0 0.0.0.255
access-list 1 permit any

int e0/0
ip access-group 1 out
```

``` bash
## R1 (Consigna 2)
access-list 2 deny 20.0.0.0 0.0.0.255
access-list permit any

int e0/0
ip access-group 2 out

## Considerar que solo se puede poner un lista por sentido (in/out).
```