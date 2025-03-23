---
tags:
  - CyberSec
  - CCNA
date created: Saturday, October 19th 2024, 11:08:14 pm
date modified: Wednesday, November 20th 2024, 11:33:50 pm
---
Algunas de las amenazas wi-fi más comunes son:
- War driving and war chalking
- WEP and WPA cracking 
- Rogue Access Point 
- Wireless Evil Twins 
#### War driving and war chalking 
Es el proceso de encontrar puntos de access point disponibles dentro de un área determinada conduciendo y escuchando señales. Esto combina monitorea y posicionamiento GPS ya que registra la ubicación de cada access point. Esta información obtenida se puede centralizar en herramientas abiertas como _Kismet_ o _InSSID_ para despues obtener una vista gráfica de la zona visualizada en mapas. 

#### WEP and WPA cracking 
Con el cifrado **WEP**, el descifrado de los datos se puede lograr utilizando _vectores de inicialización (IV)_.

Los IV son pequeña porciones de datos que están asociadas con los paquetes y que ayudan a crear una clave que puede cambiar todo el tiempo. Una clave estática junto con un valor IV puede generar una clave única siempre que los valores IV cambien cada vez que se envían datos. Estos IV se transmiten en texto plano con los datos cifrados, por lo que si un atacante logra capturar una cantidad significativa de datos podría revertir el proceso de cifrado. IV se envía en texto plano porque la estación receptora autorizada debe utilizarlo para descifrar los datos, ya que es la única parte del paquete que no se conoce. 

El problema de WEP es que el tamaño de la clave es pequeño al igual que el tamaño del IV, originalmente esta limitado a 64 bits y luego aumento a 128 bits. Estos 64 bits contenían una clave de 40 bits y un IV de 24 bits. 

Algunos valores IV generaban un cifrado más débiles que otros, los cuales algunos fabricantes evitaban esos IV disponibles. 

Una técnica que utilizan los atacantes es inyectar frames para duplicar intencionalmente los valores IV, lo que facilita el proceso de descifrado.

**WPA** es un protocolo de cifrada más avanzada que WEP. Una de sus ventajas es que ofrece un algoritmo criptográfico mejorado que cambia constantemente las claves durante la vida de una sesión. Las redes modernas utilizan WPA2 que se presentas en tres formas:
- WPA2-Personal: utilizado en redes privadas, se base en claves precompartidas (clave estática)
- WPA2-Enterprise: utilizado en redes empresariales, basado en el estándar IEEE 802.1X (las claves cambian constantemente)
- WPA3: anunciado en 2018, es una versión mejorada de WPA 

Si bien es WPA es más seguro que WEP, tambien tiene vulnerabilidades, en concreto WPA2-Personal es vulnerable a una serie de ataques que incluyen:
- Fuerza bruta, intentar cada combinación de caracteres
- Diccionario, intentar por cada palabra de un diccionario común 

#### Rogue Access Points 
Puntos de acceso no autorizados, generalmente instalados por los usuarios en una red ya sea de forma malintencionada, planificado por un atacante externo o puede que sea por error.

Se puede evitar mediante revisiones periódicas de la infraestructura, tambien el uso de [[802.1X]]  para que haya un proceso de autenticación antes de conectarse a la red.  

#### Wireless Evil Twins 
En un ataque en el que se trata de suplantar un Access Point legitimo por uno que puede ser malicioso (el mismo SSID y la misma configuración de seguridad) con objetivo de que los usuarios se conecten a falso AP, este puede aumentar el nivel de señal para aumentar las posibilidades de conexión.

Para evitar esto puede implementar una capa adicional de cifrado dentro de la red inalambrica mediante HTTPS o comunicarse a través de una VPN para que los datos cifrados estén seguros, incluso cuando se conecta a un AP que no es de confianza.