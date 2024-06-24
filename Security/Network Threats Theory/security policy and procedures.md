> Declaración formal de las reglas y pautas que deben seguir los usuarios, contratistas y empleados temporales de la organización, así com cualquier persona que tenga acceso a los datos y activos informaticos de la empresa - _RFC 2196_

Estas políticas se pueden formar a partir de un esquema modular, lo cual permite desarrollar una política separada para cada modulo o una política global. Este enfoque también se recomienda al realizar evaluciones de riesgos y [[threat]]s.

Los aspectos más importantes cubiertos por las políticas y procedimientos de seguridad son:
- Identificar los activos de la empresa.
- Determinar cómo se utilizan los activos de la organización.
- Definición de roles y responsabilidades de comunicación.
- Describir herramientas y procesos existentes.
- Definición del proceso de gestión de incidentes de seguridad.

### security policy methodology

Como primer paso se debe clasificar / identificar los activos de la empresa y asignarles un valor cuantitativo en función del impacto en su perdida. Luego determinar los [[threat]]s a esos activos, para enfocarse en esos puntos concretos. 

Luego se realiza una evalución de riesgos y vulnerabilidades para determinar que ocurran las [[threat]]s . Se pública la política de seguridad, lo cual implica implementar una mitigación rentable para proteger la org. El ultimo paso implica revisar y documentar periódicamente la política de seguridad desarrollada. 

Muchas orgs tienen plantillas para desarrollar sus políticas de seguridad, algunos componentes comunes:
- _Politica de uso aceptable_: define las funciones, responsabilidades y procesos permitidos con respecto a los dispositivos de SW / HW. Esto esta orientado al usuario final. 
- _Políticas de control de acceso a la red_: contiene principios generales del control de acceso y puede estar vinculada con aspectos como requisitos y almacenamiento de credenciales, o clasificación de los datos.
- _Política de gestión de seguridad_: resume los mecanismos de seguridad de al organización y defina fomras de gestionar la infraestructura de seguridad con las herramientas adecuadas (para dispositivos NAC)
- _Política de respuesta y manejo de incidentes_: este documento describe las políticas y procedimientos por los cuales se manejan los incidentes de seguridad 
- _Políticas de VPN_: cubre lo relacionado a las VPN y sus aspectos de seguridad que le conciernen. Se pueden aplicar diferentes políticas a usuarios remotos por ejemplo o a usuarios de VPNs site-to-site.
- _Políticas de gestión de parches_: debe cubrir  los procedimientos para parchear y mantener los sistemas existentes 
- _Política de seguridad fisica_: involucra aspectos de seguridad fisica como control de acceso (insignias, biometria y seguridad de las instalaciones)
- _Capacitación y concientización_: campañas continuas de concientización y capacitación permiten sustentar la política de seguridad en la org, esto es especialmente aplicable a los nuevos empleados 

El proceso de evalución de riesgos involucra tres componentes:
- Gravedad
- Probabilidad 
- Control 

Gravedad y probabilidad hacen referencia a la probabilidad e impacto de un determinado ataque en la organización. Control define como utilizará la política par controlar y minimizar los riesgos. 

Los tres componentes se pueden utilizar para desarrollar un índice de riesgo (RI) que utiliza la siguiente fórmula: 
$$RI\ =\ \dfrac{(factor\ de\ gravedad\ *\ factor\ de\ probabilidad)}{factor\ de\ riesgo}$$
En la cual:
- Factor de gravedad representa la pérdida cuantitativa de un activo comprometido
- Factor de probabilidad es el valor del riesgo que realmente ocurre 
- Factor de control es la capacidad de controlar y gestionar ese riesgo 

``` ad-eje
Por ejemplo, el factor de gravedad puede tener un rango de 1 a 5, el factor de probabilidad puede tener un rango de 1 a 3 y el factor de control también puede tener un rango de 1 a 3. Si observamos un ejemplo particular, si el factor de gravedad para un ataque DoS en un servidor de correo electrónico que dura dos horas tiene un valor de 3, el factor de probabilidad tiene un valor de 2 y el factor de control tiene un valor de 1, entonces el RI calculado tiene un valor de 6 (3 * 2/1 = 6). Este cálculo debe aplicarse a diferentes áreas de la red y debe tener en cuenta diferentes tipos de amenazas.

```