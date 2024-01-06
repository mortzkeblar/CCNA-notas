
Los valores del Routing Table Entry provienen de `show ip route`. 

![](_anexos_/Screenshot%20from%202023-12-27%2016-43-38.png)

### Valores importantes (CCNA) sobre la distancia por defecto
_Este punto es para remarcar estos valores por su importancia para el examen de certificación_

| Route Source                       | Default Distance |
| ---------------------------------- | ---------------- |
| Connected interface                | 0                |
| Static route out an interface      | 0                |
| Static route to a next-hop address | 1                |
| EIGRP summary route                | 5                |
| Internel EIGRP                     | 90               |
| OSPF                               | 110              |
| RIPv1, RIPv2                       | 120              |
| External EIGRP                     | 170              |
| Unknown                                   | 255                 |
