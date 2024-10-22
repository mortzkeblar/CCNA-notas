---
tags:
  - IOS
  - CCNA
---

IOS (Internetworking Operative System) es una serie de sistemas operativos propietaria de Cisco que se usan en varios de sus routers y switches. 

## Command hierarchy mode 
- EXEC mode  `Router>`, es el mode con menos privilegios de todos. Permite ver información sobre el dispositivo y el estado, no permite cambios en la configuración ni comandos demasiado intrusivos como `reload`.
- Privileged EXEC mode `Router#`, acceso ilimitado a todas las opciones de `show` comando, tambien permite realizar acciones como reiniciar el dispositivo, guardas la configuración, crear/eliminar archivos, etc.
- Global configuration mode `Router(config)#`, este modo te permite realizar configuraciones en el dispositivo. Dentro de este existen otros submodos que no son tratados en el temario. 

![[Pasted image 20241021122306.png]]

## Keyboard shortcuts 
- `Ctrl + A`, mueve el cursor al principio de la linea de comando 
- `Ctrl + E`, mueve el cursor al final de la linea de comando 
- `Ctrl + U`, elimina todos los caracteres a la izquierda del cursor 

## IOS configuration files 
Cisco IOS devices tienen dos diferentes archivos de configuración almacenados: `running-config` y `startup-config`.
- `running-config`, esta determina la configuración actual del dispositivo, se almacena en la RAM por lo cual el archivo y los cambios en este se pierden al reiniciar/apagar el equipo.
- `startup-config`, determina el estado de la configuración al momento de iniciar un equipo. Este se carga al iniciar el boot del dispositivo y se realiza una copia de este para el `running-config`, se almacena en la NVRAM (non-volatile RAM).

Tenemos varios comandos para copiar la configuración del `running-config` al `startup-config` que realizan la misma función. 

``` bash
write 
write memory 
copy running-config startup-config 
```

Para hacer un factory reset al equipo, se debe borrar el `startup-config` y reiniciar con el comando `reload`.

``` bash
write erase 
erase nvram 
erase startup-config
```

## Password-protecting privileged EXEC mode 
### enable password 
`enabled password` es una funcionalidad legacy que se utiliza para propocionar una contraseña  para acceder al privileged EXEC mode. 

![[Pasted image 20241021132614.png]]

El problema con esta contraseña es que se almacena en texto claro. La cual se puede encontrar con `show running-config | include enable`. Para solucionar esto se puede usar el comando `service password-encryption` que encripta todas las contraseñas configuradas. 

![[Pasted image 20241021132839.png]]

Este comando utiliza un cifrado _type 7_ la cual es insegura y no se recomienda. 
### enable secret 
`enable secret` es una funcionalidad que al igual que `password` pero mucho más segura en la encriptación. Ya que almacena la contraseña como un hash y no como texto claro cifrado. 

![[Pasted image 20241021133427.png]]

> Si se tiene habilitado tanto `enable password` como `enable secret`, ambos permanecen en el archivo de configuración pero se da prioridad a lo establecido en `enable secret`

Para realizar el hash se utilizan algoritmos de hashing las cuales varian según el soporte del dispositivo. P. ej. 
- Type 9, tambien llamada scrypt 
- Type 5 (MD5), usada por dispositivos más antiguos. Es más insegura que Type 9.



