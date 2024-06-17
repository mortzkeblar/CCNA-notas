---
tags:
  - CCNA
---
Este es el método de conmutación más rápido porque el frame solo se revisa para obtener la dirección MAC de destino e inmediatamente reenviar el frame al destino (si es que este se encuentra en la [[MAC address table]], o en todo caso reenviarlo mediante broadcast). 

La desventaja de este método es que no realiza verificación de errores por lo cual lo hace a la vez el menos confiable. Solo verifica si el frame tiene un tamaño menor a 64 bits (lo que se denomina _frame runt_), este se elimina (por esto además este método es conocido como _runt-free switching_).

### related
- [[store-and-forward]] 
- [[fragment-free]] 