Ver las instrucciones de una `access-list`
- Este comando tambien permite ver la cantidad de veces que un solicitud hizo `match` con alguna instrucción.
``` bash
R1$ show access-list
Extended IP access list SEC_RED10
...
30 deny icmp host 10.0.0.2 30.0.0.0 0.0.0.255 echo (5 matches)
...

```

Borrar una instrucción de una `access-list`
``` bash
## Borramos la instrucción asignada '30'
R1$ conf t
R1(config)$ ip access-list extended SEC_RED10
R1(config-ext-nacl)$ no 30
```

Agregar una instrucción en una posición especifica
``` bash
# R1(config-ext-nacl)$ <NUMBER_POSITION> <ACL instruction> 
R1(config-ext-nacl)$ 30 deny host 10.0.0.2 30.0.0.0 0.0.0.254 echo
```