# PNAC (Port-Based Network Access Control) - IEEE 802.1x (Dot1x)




> Situación: cuando nos conectamos a una red a traves de un cable (por ej), no tenemos metodos de autenticación y cualquiera podria tener acceso a la L2 de la red. El riesgo es que si un actor malicioso esta conectado podria tratar de _sniffear_ o interceptar el trafico en la red con ataques tipo VLAN hopping attack entre otros. 

IEEE 802.1x se encarga de otorgar autenticación dentro de la red. Esto se compone de tres actores.
- Supplicant, el solicitante para ingresar a la red
- Authenticator, el dispositivo que permite el ingreso o no del supplicant
- Authentication Server, es el dispositivo que analiza la solicitud que le envia authenticator

## EAPOL
_Ver: [PNAC - EAPOL](PNAC%20-%20EAPOL.md)_

### Configuración
_Ver: [PNAC - Configuration](PNAC%20-%20Configuration.md)_

