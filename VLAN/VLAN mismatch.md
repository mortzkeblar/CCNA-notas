---
tags:
  - configuration
  - problem
  - VLAN
---

Si en el switch cambian la `VLAN nativa` a otro valor diferente de 1.

``` bash
SW1(config-if)$ switchport trunk native vlan 30
```

Nos debería saltar un error de tipo `VLAN mismatch`. La solución a este tipo de problema es configurar la `VLAN nativa` del otro lado (en este caso del router) al mismo valor. Es decir, 30. 

``` bash
R1(config)$ int e0/0.30
R1(config)$ encapsulation dot1q 30 native
```