- Es necesario identificar el `sentido del trafico` a la hora de crear un _Security Police_.
	- En un interfaz especifica (i.e. `e0/0`), la politica se aplica para los paquetes de entrada o salida?

_Ver: [ACL Standard - IN OUT](ACL%20Standard%20-%20IN%20OUT.md)_

> Importante: si planteamos esta situación. Un host tratando de conectarse a un router por telnet. Lo hace a través de una IP la cual tiene un denegación por `access-list`. 
> 
> Pero el host entonces podría conectarse al router haciendo uso de otra IP que este en otra interfaz la cual no tenga `access-list`.
>
> Para más detalles ver este [video](https://youtu.be/pTCdTwTXhMo?list=PL2A7l6PiV52esSwosIAO86zf0RGe2pjTZ&t=2080)  