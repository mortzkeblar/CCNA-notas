---
tags:
  - CyberSec
  - CCNA
date created: Saturday, October 19th 2024, 11:08:14 pm
date modified: Wednesday, November 20th 2024, 11:33:50 pm
---
Las aplicaciones tiene diferentes áreas de almacenamiento en memoria llamado buffers, si intentar almacenar más información que el tamaño del buffer este puede desbordar (hacer overflow) a áreas de memoria adyacentes a las que no se puede acceder. El atacante puede aprovecha eso y escribir código malicioso en determinadas  áreas de la memoria para que se ejecute. 

Aunque se descubra un buffer overflow, no necesariamente representa una vulnerabilidad a ser aprovechada por un atacante, se necesita un estudio extenso para poder determinar si ese desborde es vulnerable a algún ataque. 