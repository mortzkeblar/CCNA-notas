---
tags:
  - CCNA
date created: Saturday, October 19th 2024, 11:08:14 pm
date modified: Wednesday, November 20th 2024, 11:33:50 pm
---
También llamado _modified cut-through / runt-free_, este es un método intermedio entre [[cut-through]] y [[store-and-forward]], examina los frames en busca de errores pero no almacena todo su información, lo que resulta en mayor velocidad manteniendo la confiabilidad. 

Para la verificación se examinan los primeros 64 bytes de un frame (que son los más propensos a errores), si no se detecta ningun error se reenvia el frame. Al igual que [[cut-through]], se descartan los frames con tamaño menor a 64 bytes. 

### related 
- [[cut-through]]
- [[store-and-forward]] 