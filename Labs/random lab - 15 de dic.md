![[Pasted image 20241215182354.png]]

Tambien se tiene configuraciones adicionales como los puertos trunk/access en los switches y los tuneles GRE pero no esta en la conf que adjunto abajo.

```bash
-------------------------------- ISP ROUTERS ------------------------------------------

IOS5

conf t
int g0/2
ip address 30.1.1.1 255.255.255.252
int g0/3
ip address 40.1.1.1 255.255.255.252
int g0/1
ip address 10.1.1.37 255.255.255.252
int g0/0
ip address 10.1.1.33 255.255.255.252
int range g0/0-3
no sh
end
sh ip int br
wr

IOS3

conf t
int g0/7
ip address 10.1.1.11 255.255.255.248
no sh
int g0/1
ip address 10.1.1.34 255.255.255.252
no sh
end
sh ip int br
wr

IOS8

conf t
int g0/7
ip address 10.1.1.13 255.255.255.248
no sh
int g0/1
ip address 10.1.1.38 255.255.255.252
no sh
end
sh ip int br
wr


IOS1

conf t
int g0/7
ip address 10.1.1.10 255.255.255.252
no sh
int g0/1
ip address 10.1.1.1 255.255.255.252
no sh
end
sh ip int br
wr


IOS2

conf t
int g0/7
ip address 10.1.1.12 255.255.255.248
no sh
int g0/1
ip address 10.1.1.5 255.255.255.252
no sh
end
sh ip int br
wr


IOS4

conf t
int g0/0
ip address 10.1.1.2 255.255.255.252
int g0/1
ip address 10.1.1.6 255.255.255.252
int g0/2
ip address 20.1.1.1 255.255.255.252
int g0/3
ip address 21.1.1.1 255.255.255.252
int range g0/0-3
no sh
end
sh ip int br
wr


IOS7

conf t
int g0/0
ip address 20.1.1.2 255.255.255.252
int g0/1
ip address 100.0.0.1 255.255.255.248
int range g0/0-1
no sh
end
sh ip int br
wr


IOS6

conf t
int g0/0
ip address 21.1.1.2 255.255.255.252
int g0/1
ip address 181.0.0.1 255.255.255.248
int range g0/0-1
no sh
end
sh ip int br
wr


IOS9

conf t
int g0/0
ip address 30.1.1.2 255.255.255.252
int g0/1
ip address  150.0.0.1 255.255.255.248
int range g0/0-1
no sh
end
sh ip int br
wr


IOS10

conf t
int g0/0
ip address 40.1.1.2 255.255.255.252
int g0/1
ip address 175.0.0.1 255.255.255.248
int range g0/0-1
no sh
end
sh ip int br
wr

####### OSPF ############

IOS9

conf t
int lo0
ip address 1.1.1.9 255.255.255.255
router ospf 2000
passive-interface g0/1
network 30.1.1.2 0.0.0.3 area 0
network 150.0.0.1 0.0.0.7 area 0

IOS10

conf t
int lo0
ip address 1.1.1.10 255.255.255.255
router ospf 2000
passive-interface g0/1
network 175.0.0.1 255.255.255.248 area 0
network 40.1.1.1 255.255.255.252 area 0 



IOS5

conf t
ip route 150.0.0.0 255.255.255.248 g0/2 30.1.1.2   # proof of route selection
ip route 175.0.0.0 255.255.255.248 g0/3 40.1.1.2

int lo0
ip address 1.1.1.5 255.255.255.255
router ospf 2000
network 30.1.1.1 0.0.0.3 area 0
network 40.1.1.1 0.0.0.3 area 0
network 10.1.1.37 0.0.0.3 area 0
network 10.1.1.33 0.0.0.3 area 0


IOS3

conf t
int lo0
ip address 1.1.1.3 255.255.255.255
router ospf 1000
network 10.1.1.34 0.0.0.3 area 0
network 10.1.1.11 0.0.0.7 area 0

IOS8

conf t
int lo0
ip address 1.1.1.8 255.255.255.255
router ospf 1000
network 10.1.1.38 0.0.0.3 area 0
network 10.1.1.13 0.0.0.7 area 0

IOS11

conf t
int lo1
ip address 1.1.1.11 255.255.255.255
router ospf 1000
network 10.1.1.10 0.0.0.7 area 0
network 10.1.1.1 0.0.0.3 area 0

IOS2

conf t
int lo1
ip address 1.1.1.2 255.255.255.255
router ospf 1000
network 10.1.1.5 0.0.0.3 area 0 
network 10.1.1.12 0.0.0.7 area 0

IOS4

conf t
int lo0
ip address 1.1.1.4 255.255.255.255
int range g0/0-3
ip ospf 2000 area 0 


IOS6

conf t
int lo0
ip address 1.1.1.6 255.255.255.255
router ospf 2000
passive-interface g0/1
network 181.0.0.1 0.0.0.7 area 0
network 21.1.1.2 0.0.0.3 area 0 

IOS7

conf t
int lo0
ip address 1.1.1.7 255.255.255.255
router ospf 2000
passive-interface g0/1
network 20.1.1.2 0.0.0.3 area 0
network 100.0.0.1 0.0.0.7 area 0












-------------------------------- CUSTOMERS ROUTERS ------------------------------------------

------------------- VLANs and HSRP --------------------
ROD11

conf t
int g0/0
ip add 100.0.0.5 255.255.255.248
no sh
int g0/1
no sh

int g0/1.100
encapsulation dot1q 100
ip address 192.168.100.2 255.255.255.0
standby version 2
standby 100 ip 192.168.100.1
standby 100 priority 105
standby 100 preempt
no sh

int g0/1.101
encapsulation dot1q 101
ip address 172.16.0.102 255.255.255.0
standby version 2
standby 101 ip 172.16.0.101
standby 101 priority 105
standby 101 preempt
no sh

int g0/1.102
encapsulation dot1q 102
ip address 10.10.10.2 255.255.255.0
standby version 2
standby 102 ip 10.10.10.1
no sh



ROD12

conf t
int g0/0
ip add 181.0.0.10 255.255.255.248
no sh

int g0/1
no sh

int g0/1.100
encapsulation dot1q 100
ip address 192.168.100.3 255.255.255.0
standby version 2
standby 100 ip 192.168.100.1
no sh

int g0/1.101
encapsulation dot1q 101
ip address 172.16.0.103 255.255.255.0
standby version 2
standby 101 ip 172.16.0.101
no sh


int g0/1.102
encapsulation dot1q 102
ip address 10.10.10.3 255.255.255.0
standby version 2
standby 102 ip 10.10.10.1
standby 102 priority 105
standby 102 preempt
no sh


ROD21

conf t
int g0/0
ip addr 150.0.0.3 255.255.255.248
no sh
int g0/1
ip addr 175.0.0.3 255.255.255.248
no sh

int g0/2
no sh
int g0/2.20
encapsulation dot1q 20
ip addr 192.168.20.3 255.255.255.0
int g0/2.30
encapsulation dot1q 30
ip addr 172.20.20.3 255.255.255.0
int g0/2.40
encapsulation dot1q 40
ip addr 10.20.20.3 255.255.255.0


----------------- NAT -------------------

ROD11

int g0/0
ip nat outside
int g0/1.100
ip nat inside
int g0/1.101
ip nat inside
int g0/1.102
ip nat inside
exit

ip access-list standard NAT11
permit 192.168.100.0 0.0.0.255
permit 172.16.0.0 0.0.0.255
permit 10.10.10.0 0.0.0.255
exit

ip nat inside source list NAT11 interface g0/0 overload

ip route 0.0.0.0 0.0.0.0 100.0.0.1




ROD12

int g0/0
ip nat outside
int g0/1.100
ip nat inside
int g0/1.101
ip nat inside
int g0/1.102
ip nat inside
exit

ip access-list standard NAT12
permit 192.168.100.0 0.0.0.255
permit 172.16.0.0 0.0.0.255
permit 10.10.10.0 0.0.0.255
exit

ip nat inside source list NAT12 interface g0/0 overload

ip route 0.0.0.0 0.0.0.0 181.0.0.1


ROD21

conf t
int range g0/0-1
ip nat outside 
int g0/2.20
ip nat inside
int g0/2.30
ip nat inside
int g0/2.40
ip nat inside
exit

ip access-list standard NAT21
permit 192.168.20.0 0.0.0.255
permit 172.20.20.0 0.0.0.255
permit 10.20.20.0 0.0.0.255
exit

ip nat inside source list NAT21 interface g0/0 overload
ip nat inside source list NAT21 interface g0/1 overload

















-------------------------------- RANDOm ------------------------------------------------------
IOS1

conf t
int g0/0
ip address  255.255.255.252
int range g0/
no sh
end
sh ip int br
wr


IOS1

conf t
int g0/0
ip address  255.255.255.252
int range g0/
no sh
end
sh ip int br
wr




```