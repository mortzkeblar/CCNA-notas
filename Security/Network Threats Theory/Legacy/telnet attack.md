---
tags:
  - CyberSec
  - CCNA
date created: Saturday, October 19th 2024, 11:08:14 pm
date modified: Wednesday, November 20th 2024, 11:33:50 pm
---
Programas como telnet o FTP emplean autenticación basada en el usuario, el problema es que las credenciales se envia en texto claro sin cifrado por la red. Lo cual permite que un atacante pueda interceptar y obtener esas credenciales. 

Esto tambien pasa con protocolos antiguos no seguros como _rlogin, rcp_ o _rsh_ para acceder a diferentes dispositivos / sistemas. Para protegerse se deben usar protocolos seguros como _SSH_ o _SFTP_. 