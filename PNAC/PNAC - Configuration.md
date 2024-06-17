---
tags:
  - PNAC
  - CCNA
---

# Configuración de PNAC

``` bash
Switch(config)$ aaa new-model
Switch(config)$ username admin password 1234
Switch(config)$ radius-server host 170.66.23.56 auth-port 1812 acct-port 1813 key CLAVERADIUS
Switch(config)$ aaa authentication dot1x dafault group radius
Switch(config)$ dot1x system-auth-control # Habilita las funciones de auth

Switch(config)$ interface f0/14
Switch(config-if)$ switchport mode access
Switch(config-if)$ dot1x port-control auto
Switch(config-if)$ interface f0/15
Switch(config-if)$ switchport mode access
Switch(config-if)$ dot1x port-control auto

## Para ver si una interfaz tiene acceso a la red
Switch$ show dot1x interface f0/14

```

- _AAA_: authentication, authorization and accounting (autenticación, autorización y responsabilidad)
- _auth-port_: puerto de _authentication_
- _acct-port_: puerto de _accounting_

![](_anexos_/Screenshot%20from%202024-01-05%2008-23-19.png)