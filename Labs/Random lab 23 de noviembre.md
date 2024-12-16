---
tags:
  - CCNA
---
![[ospf_diagram_yed.svg]]


### IP address configuration 
``` 
# R0-2
en
conf t
int g0/0
ip address 10.0.254.18 255.255.255.252 
no sh 

int g0/1
ip address 10.0.254.22 255.255.255.252 
no sh 

int g0/2
no sh
int g0/2.10
encapsulation dot1q 10
ip address 192.168.10.1 255.255.255.0

int g0/2.20
encapsulation dot1q 20
ip address 192.168.20.1 255.255.255.0

int g0/2.30
encapsulation dot1q 30
ip address 192.168.30.1 255.255.255.0

end
wr


# RO-3

en
conf t
int g0/0
ip address 10.0.254.14 255.255.255.252 
no sh 

int g0/1
ip address 10.0.254.21 255.255.255.252 
no sh 

end
wr

# RO-1

en
conf t
int g0/0
ip address 10.0.254.13 255.255.255.252 
no sh 

int g0/1
ip address 10.0.254.17 255.255.255.252 
no sh 

int g0/2
ip address 10.0.254.6 255.255.255.252 
no sh 

int g0/3
ip address 10.0.254.2 255.255.255.252 
no sh 

end
wr

# RO-4

en
conf t
int g0/0
ip address 10.0.254.5 255.255.255.252 
no sh 

int g0/1
ip address 10.0.254.9 255.255.255.252 
no sh 

end
wr


# RO-5

en
conf t
int g0/0
ip address 10.0.254.1 255.255.255.252 
no sh 

int g0/1
ip address 10.0.254.10 255.255.255.252 
no sh 

int g0/2
ip address 10.0.254.25 255.255.255.252 
no sh 

end
wr


# RO-6

en
conf t
int g0/0
ip address 10.0.254.26 255.255.255.252 
no sh 

int g0/1
ip address 10.0.254.29 255.255.255.252 
no sh 

end
wr

# RO-7

en
conf t
int g0/0
ip address 10.0.254.30 255.255.255.252 
no sh 

int g0/1
ip address 10.0.254.33 255.255.255.252 
no sh 

end
wr

# RO-8

en
conf t

int g0/0
ip address 10.0.254.34 255.255.255.252 
no sh 

end
wr
```

### OSPF configuration 

``` 
# RO-2 

router ospf 1
 network 192.168.10.0 0.0.0.255 area 0
 network 192.168.20.0 0.0.0.255 area 0
 network 192.168.30.0 0.0.0.255 area 0

# RO-1 

router ospf 1
 network 10.0.254.0 0.0.0.7 area 0 # unnecessary
 network 10.0.254.0 0.0.0.31 area 0

# RO-3

router ospf 1
network 10.0.254.14 0.0.0.0 area 0 
network 10.0.254.21 0.0.0.0 area 






```
