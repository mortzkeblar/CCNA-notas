# Mitigación

Al configurar el switch con DHCP snooping, todos los puertos quedan en modo UNTRUST. Solo los puertos TRUST podrán enviar mensajes DISCOVER (Broadcast) y responder los OFFER.
- Los broadcast no se anulan, puesto que es indispensable para poder tener DHCP en la red
- Lo que se bloquea son las respuesta unicast (que generalmente son los de tipo OFFER) de cualquier interface que no sea el del DHCP server legitimo

``` bash
S1(config)$ ip dhcp snooping
S1(config)$ int f0/20
S1(config-if)$ ip dhcp snooping trust # Configuramos el servidor legitimo en f0/20

```

DHCP Snooping mantiene una base de datos llamada DHCP _Snooping Binding Table_ donde se incluye información de todos los puertos en modo UNTRUST
![](_anexos_/Screenshot%20from%202024-01-05%2009-08-37.png)
