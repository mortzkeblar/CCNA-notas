---
tags:
  - CCNA
---

![](../_anexos_/Screenshot%20from%202024-01-01%2009-57-09.png)

### Descripción de la topología
![](../_anexos_/Screenshot%20from%202024-01-01%2010-07-46.png)

#### Consigna

![](../_anexos_/Screenshot%20from%202024-01-01%2010-08-33.png)
![](../_anexos_/Screenshot%20from%202024-01-01%2012-16-10.png)


> Recordad: cuando se este utilizando `access-list` con NAT. No quiere decir que estemos utilizando el router como un `firewall`, sino más bien seleccionando que host son lo que pueden salir a internet.

#### Solución

``` bash
# Solución Nro. 2
R1$ conf t
R1(config)$ ip access-list standard NAT 
R1(config-std-nacl)$ permit 10.0.0.0 0.255.255.255 
R1(config-std-nacl)$ exit

R1(config)$ ip nat pool UNINET 88.177.23.2 88.177.23.20 netmask 255.255.255.0 

R1(config)$ ip nat inside source list NAT pool UNINET overload

R1(config)$ int f1/1
R1(config-if)$ ip nat outside
R1(config-if)$ int g0/0
R1(config-if)$ ip nat inside
R1(config-if)$ int f1/0
R1(config-if)$ ip nat inside 


## En este punto TynyCore-1 deberia poder enviar ping a ISP a través de 88.177.23.1 en f1/0

R1(config)$ show ip nat translations

```

``` bash
# Solución Nro. 3
R5$ conf t
R5(config)$ ip access-list standard NAT 
R5(config-std-nacl)$ permit 192.168.10.0 0.0.0.255
R5(config-std-nacl)$ permit 192.168.20.0 0.0.0.255
R5(config-std-nacl)$ exit

R5(config)$ ip nat inside source list NAT interface f1/0 overload

R5(config)$ int f1/0
R5(config-if)$ ip nat outside
R5(config-if)$ int f1/1.10
R5(config-if)$ ip nat inside
R5(config-if)$ int f1/1.20
R5(config-if)$ ip nat inside

## Para permitir el ping desde PC1 en XYZ.COM hacia R1 en f1/1
### En R5
R5(config)$ ip route 0.0.0.0 0.0.0.0 100.224.141.9
### En R1
R1(config)$ ip route 0.0.0.0 0.0.0.0 88.177.23.1

```

``` bash
# Solución Nro. 4
R4(config)$ ip access-list standard NAT
R4(config-std-nacl)$ permit 192.168.10.0 0.0.0.255
R4(config-std-nacl)$ exit

R4(config)$ ip nat inside source list NAT interface g0/0 overload

R4(config)$ int g0/0 
R4(config-if)$ ip nat outside
R4(config-if)$ int f1/0
R4(config-if)$ ip nat inside

## Acceder al ISP
R4(config)$ ip route 0.0.0.0 0.0.0.0 199.43.128.193

## Arreglamos las rutas de R2 y R3 para indicarle que la ruta por R1 va a internet
R1(config)$ router ospf 1
R1(config-router)$ default-information originate

## Configuramos un NAT estatico para asignar una IP publica a WEBSERVER
R4(config)$ ip nat inside source static 192.168.10.4 199.43.128.195
```


![](../_anexos_/Screenshot%20from%202024-01-01%2012-54-51.png)