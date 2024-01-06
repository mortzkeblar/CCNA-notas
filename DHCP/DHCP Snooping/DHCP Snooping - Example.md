_Ver: [DHCP Snooping - Mitigation](DHCP%20Snooping%20-%20Mitigation.md)

![](_anexos_/Screenshot%20from%202024-01-05%2009-11-03.png)

En un ejemplo más complejo nos preguntamos, _donde deberian ir las interfaces trust y untrust?_
Para esto, seguimos una serie de reglas
- Todos los puertos que conectan con dispositivos de acceso, van en modo untrust.
	- Esto inhabilita que un dispositivo final pueda llegar al switch y montar un falso DHCP
- Los servers que no sean el DHCP ligitimo tambien van bloqueadas, porque siempre existe la posibilidad de que un atacante pueda ingresar a ese server y salir desde ahi el DHCP falso.
	- Por esto, el servidor web no va a ser capaz de enviar mensajes unicast de tipo OFFER