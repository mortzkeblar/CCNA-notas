---
tags:
  - routing
  - static
  - example
---

La configuración en IPv6 es similar a IPv4 (ver: static configuration). La diferencia principal es:
- Utilizar network prefix (prefijo de red) en vez de subnet mask (mascaras de red). 
- IPv6 es desahabilitado por defecto en los routers Cisco 

``` bash
_R1(config)$ ipv6 unicast-routing_

_R1(config)$ int f0/0_

_R1(config-if)$ ipv6 enable_

_R1(config-if)$ no shut_

_R1(config-if)$ exit_

_R1$ show ipv6 interface brief_
_FastEthernet0/0            [up/up]_
_FE80::C000:8FF:FE01:0_
_FastEthernet0/1            [administratively down/down]_
You can now configure the IPv6 addresses on R2:

_R2$ conf t_
_Enter configuration commands, one per line.  End with CNTL/Z._

_R2(config)$ ipv6 uni_

_R2(config)$ int f0/0_

_R2(config-if)$ ipv6 enable_

_R2(config-if)$ no shut_

_R2(config-if)$ int lo0_

_R2(config-if)$ ipv6 address fec0::1/16_

_R2(config-if)$ end_

_R2$ show ipv6 int brief_
_FastEthernet0/0            [up/up]_
_FE80::C001:8FF:FE01:0_
_FastEthernet0/1            [administratively down/down]_
_Loopback0                  [up/up]_
_FE80::C001:8FF:FE01:0_
_FEC0::1_

You can see that there is no route to the FEC0 network on R1:
_R1$ show ipv6 route_
_IPv6 Routing Table – 1 entries_
_Codes: C – Connected, L – Local, S – Static, R – RIP, B – BGP,_
_U – Per-user Static route, M – MIPv6,_
_I1 – IS-IS L1, I2 – IS-IS L2, IA – IS-IS interarea,_
_IS – IS-IS summary, O – OSPF intra, OI – OSPF inter,_
_OE1 – OSPF ext 1, OE2 – OSPF ext 2, ON1 – OSPF NSSA ext 1,_
_ON2 – OSPF NSSA ext 2, D – EIGRP, EX – EIGRP external_
_L   FF00::/8 [0/0]_
_via ::, Null0_

A useful feature to determine the remote IPv6 address (if you don’t have an accurate diagram or access to the device) is CDP, which was already discussed briefly. You will need to use the detail tag:
_R1$ show cdp nei detail_
_————————-_
_Device ID: R2.lab.local_
_Entry address(es):_
_IPv6 address: **FE80::C001:8FF:FE01:0**  (link-local)_
_Platform: Cisco 3725, Capabilities: Router Switch IGMP_
_Interface: FastEthernet0/0, Port ID (outgoing port): FastEthernet0/0_
_Holdtime: 176 sec_

[output truncated]

You can ping this address to check connectivity:
_R1$ ping ipv6 FE80::C001:8FF:FE01:0_
_Output Interface: FastEthernet0/0_
_Type escape sequence to abort._
_Sending 5, 100-byte ICMP Echos to FE80::C001:8FF:FE01:0, timeout is 2 seconds:_
_Packet sent with a source address of FE80::C000:8FF:FE01:0_
_!!!!!_
_Success rate is 100 percent (5/5), round-trip min/avg/max = 16/22/40 ms_

It should be no surprise that the next ping fails because there is no route to it:
_R1$ ping ipv6 FEC0::1_
_Type escape sequence to abort._
_Sending 5, 100-byte ICMP Echos to FEC0::1, timeout is 2 seconds:_
_….._
_Success rate is 0 percent (0/5)_

You need to add a route to reach the network. If you are doing this on a broadcast network (such as Ethernet), you also need to specify an exit interface because R2 will not respond to a neighbor solicitation for a network or host it has a route to. If it was a Serial link, you could just specify the destination network and next-hop (link-local) address. Your link-local address may well differ from mine, so please check.
_R1(config)$ ipv6 route fec0::/64 FastEthernet0/0 FE80::C001:8FF:FE01:0_

_R1(config)$ end_

_R1$ ping ipv6 FEC0::1_
_Type escape sequence to abort._
_Sending 5, 100-byte ICMP Echos to FEC0::1, timeout is 2 seconds:_
_!!!!!_
_Success rate is 100 percent (5/5), round-trip min/avg/max = 12/20/28 ms_

_R1$ show ipv6 route_
_IPv6 Routing Table – 2 entries_
_Codes: C – Connected, L – Local, S – Static, R – RIP, B – BGP,_
_U – Per-user Static route, M – MIPv6,_
_I1 – IS-IS L1, I2 – IS-IS L2, IA – IS-IS interarea,_
_IS – IS-IS summary, O – OSPF intra, OI – OSPF inter,_
_OE1 – OSPF ext 1, OE2 – OSPF ext 2, ON1 – OSPF NSSA ext 1,_
_ON2 – OSPF NSSA ext 2, D – EIGRP, EX – EIGRP external_
_S   **FEC0::/64** [1/0]_
_**via FE80::C001:8FF:FE01:0, FastEthernet0/0**_
_L   FF00::/8 [0/0]_
_via ::, Null0_
```



