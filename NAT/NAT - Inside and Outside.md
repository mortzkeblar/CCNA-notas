---
tags:
  - NAT
  
---


> Las diferencias generalmente estan en torno a la *perpectiva* del usuario que analiza su situación.
> - Red interna LAN (`inside`)
> - Red externa WAN (`outside`)


![](../_anexos_/Screenshot%20from%202024-01-01%2009-18-35.png)

- **Inside local:** es la dirección del host ubicado en la _inside_, segun como se ve desde la _inside_. (Es la IP address del host interno)
- **Inside global:** es la dirección dle host ubicado en la _inside_, pero segun como se ve desde la _outside_ (Es la IP publica que esta siendo utilizada como NAT) 

- **Outside local:** es la dirección del host ubicado en la _outside_ (PC2) tal como se ve desde la _inside_.
- **Outside global:** es la dirección del host ubicado en la _outside_ (PC2) tal como se ve desde la _outside_. 

``` bash
R1$ show ip nat translations
Pro  Inside global        Inside local        Outside local     Outside global
tcp  66.178.221.10:23781  192.168.0.10:23781  200.23.101.44:80  200.23.101.44:80
```

### Examples

_Ver: [NAT - Inside and Outside - Examples](NAT%20-%20Inside%20and%20Outside%20-%20Examples.md)_