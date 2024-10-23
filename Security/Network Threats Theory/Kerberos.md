---
tags:
  - CCNA
---
Kerberos es un [[user authentication]] protocol que permite a un usuario ingresar sus credenciales una vez y recibir acceso a todos los recursos de red necesarios  sin necesidad de volver a autenticarse contra cada uno de ellos. Ofrece funciones criptográficas avanzada utilizando authentication mutua entre el cliente - servidor para proteger de ataques MITM. 

Fue desarrollado por el MIT y esta cubierto en el _RFC 4120_. Microsoft utiliza Kerberos como metodo de autenticación desde Windows 2000.

Kerberos tiene tres componentes principales:

- *KDC (Centro de distribución de claves)* : el KDC verifica las credenciales de inicio de sesión válidas y garantiza la identidad del usuario. Opera en el puerto TCP y UDP 88.
- *Servicio de autenticación* : este es el componente que realmente realiza la autenticación en la red.
- *Servicio de concesión de tickets* : Kerberos funciona con tickets internos y este servicio es el componente que gestiona los tickets y proporciona acceso a los usuarios en los diferentes componentes de la red.


``` ad-eje
A continuación analizaremos un ejemplo en el que un usuario quiere acceder a un servidor de aplicaciones. Una vez que el usuario decide iniciar sesión en la red, deberá hablar con el Servicio de autenticación enviando una solicitud de inicio de sesión. Durante este proceso, la fecha y la hora en la computadora local se cifran utilizando la clave que se encuentra en el hash de la contraseña. Debido a que el hash de la contraseña en realidad no se envía a través de la red, se utiliza como clave para cifrar la fecha y la hora. Por este motivo, todos los usuarios de la red deberían utilizar NTP para sincronizar sus relojes.

El Servicio de Autenticación recibe la información enviada por el usuario e intenta descifrar la información con el hash de las credenciales que posee. Una vez descifrado correctamente, envía un ticket de concesión de ticket (TGT) al usuario, que incluye:

- Nombre del cliente
- Dirección IP
- Marca de tiempo
- Período de validez del TGT (predeterminado 10 horas)

El TGT está cifrado con la clave secreta de KDC, por lo que nadie más puede descifrarlo. El cliente también recibirá una clave de sesión del Servicio de concesión de billetes (TGS), que se utiliza para la comunicación entre el cliente y el TGS. Luego, el TGS se cifra con el hash de contraseña del usuario para que pueda descifrarlo.

Una vez que el cliente tiene un ticket que permite el acceso a los recursos, enviará el ticket y el nombre del servidor de aplicaciones al que desea acceder al TGT para solicitar acceso a ese servidor específico. Esta solicitud en particular tiene una marca de tiempo con el ID del cliente y se cifra con la clave de sesión TGS (para evitar la suplantación de solicitudes). El TGS enviará una respuesta al cliente con la siguiente información:

- Una clave de sesión para usar con el servidor de aplicaciones (esto también está cifrado con la clave de sesión TGS)
- Un ticket de servicio que contiene información del usuario y clave de sesión de servicio (cifrada con la clave secreta del servidor de aplicaciones)

Cuando el cliente recibe información, no puede descifrarla, por lo que necesita pasar el ticket de servicio cifrado al servidor de aplicaciones. El cliente también enviará una autenticación con marca de tiempo, cifrada con la clave de sesión del servicio. Una vez que el servidor de aplicaciones recibe esta información, comienza a descifrar los datos para confirmar su integridad. El servidor puede enviar un mensaje final de verificación al usuario para asegurarse de que no haya ningún intermediario. Este es un paso opcional que se implementa con mayor frecuencia en entornos de alta seguridad. Finalmente, el cliente recibe acceso a los recursos del servidor de aplicaciones.
```