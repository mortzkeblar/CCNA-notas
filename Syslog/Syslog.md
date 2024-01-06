![](_anexos_/Screenshot%20from%202024-01-02%2000-54-22.png)

Debido a que los dispositivos Cisco, a diferencia de un server, tienen muy poca capacidad de almacenamiento, existen dos alternativas para administrar los mensajes de registro.
- _Buffer interno:_ ubicado en la RAM (se pierde en cada reinicio) y tiene un tamaño de unos pocos kilobytes (habilitado por defecto).
- _Syslog:_ utilizar el cliente SYSLOG integrado de Cisco de tipo UNIX para enviar los mensajes de un servidor remoto donde se podrán almacenar (no habilitado por defecto).

##### Kiwi Syslog Server
https://www.kiwisyslog.com/products/kiwi-syslog-server/product-overview.aspx
##### TFTPD32
https://tftpd32.jounin.net

### Log levels

![](_anexos_/Screenshot%20from%202024-01-02%2000-55-45.png)

### Configuración
_Ver: [Syslog - Configuration](Syslog%20-%20Configuration.md)_