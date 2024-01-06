
``` bash
interface GigabitEthernet1/0/1

interface GigabitEthernet1/0/2

interface GigabitEthernet2/0/1

interface GigabitEthernet2/0/1
```

Analizamos `GigabitEthernet1/0/2`
- _Unit number_: el numero de la unidad en el stack, Este numero junto al numero de la interfaz identifica completamente el puerto. En nuestro ejemplo `g1/0/2` es el puerto dos en la primera unidad del stack 
- _Slot number_: el numero del slot siempre es 0
- _Interface number_: puerto, LAG, tunnel o VLAN ID. Para nuestro ejemplo identifica al puerto 2

