---
tags:
  - CCNA
---
UDP (User Diagram Protocol) envia el trafico al maximo esfuerzo posible, no un control de flujo como TCP ni garantiza la fiabilidad de los datos enviados. Por lo cual se le considera un protocolo no orientado a conexión.
- No realiza el control basado en secuenciación para asegurar que los segmentos de datos se procesen en el orden correcto y sin perdidas. 
- El host receptor no envía acknowledgments hacia el emisor por si existen fallas en el envío. 
- El control de errores va a depender de los protocolos que estén corriendo en las capas superiores

![[Pasted image 20241011205639.png]]