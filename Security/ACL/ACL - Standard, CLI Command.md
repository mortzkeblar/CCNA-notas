_Ver: [ACL - Standard, IN OUT](ACL%20-%20Standard,%20IN%20OUT.md)_
### Standard ACL numbered
- `1-99; 1300 a 1999`
- `permit/deny`: política

``` bash
R1(config)$ access-list 1 remark 'Permite salida al administrador'
R1(config)$ access-list 1 permit 172.16.0.2 0.0.0.0
R1(config)$ access-list 1 permit 172.16.0.3 0.0.0.0
R1(config)$ access-list 1 remark 'Permite salida al servidor'
R1(config)$ access-list 1 permit 172.16.0.128 0.0.0.1
```
- Considerar que cada número es una lista, entonces podemos asignar varias instrucciones a una sola lista, por ejemplo a `1`.
### Standard ACL named
- Origen del paquete IP
	- `<IP address> <Wildcard>`, para seleccionar un rango de IPs.
	- `host <IP address>`. para seleccionar una IP especifica.
	- `any`, para seleccionar todas las IPs posibles.
- Es recomendable que el nombre de la lista este en mayuscula, asi se puede ver con claridad al usar `show ip running-config`.
- `permit/deny`: política

``` bash
R1(config)$ ip access-list standard RESTRINGIR_SALIDA
R1(config-std-nacl)$ remark 'permite salida del administrador'
R1(config-std-nacl)$ permit 172.16.0.2 0.0.0.0
R1(config-std-nacl)$ permit 172.16.0.3 0.0.0.0
R1(config-std-nacl)$ remark 'permite salida del servidor'
R1(config-std-nacl)$ permit 172.16.0.128 0.0.0.1
```

`Cada una de esas instrucciones, es un ACE. Un ACL esta compuesto de multiples ACE.` 
