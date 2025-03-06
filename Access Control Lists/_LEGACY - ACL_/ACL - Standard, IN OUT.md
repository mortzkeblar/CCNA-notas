---
tags:
  - ACL
  - CCNA
date created: Saturday, October 19th 2024, 11:08:14 pm
date modified: Wednesday, November 20th 2024, 11:33:50 pm
---

Una vez creadas las ACL, se deben aplicar en una interfaz en sentido IN o OUT. Normalmente las standard ACL se aplican lo más cerca del destino y las extended ACL lo más cerca del origen. Para ellos se utiliza `ip access-group` dentro de la  interfaz que se quiera configurar. 
``` bash
R1(config)$ interface f0/0
R1(config)$ ip access-group 1 in
```

> Es importante elegir correctamente la interfaz (ya sea de entrada o salida) para no bloquear o permitir trafico que no esta contemplado.

#####  References
- https://youtu.be/nq1SoUM6XA0?list=PL2A7l6PiV52esSwosIAO86zf0RGe2pjTZ
