---
tags:
  - IOS
  
---

![](../_anexos_/Screenshot%20from%202024-01-02%2001-31-47.png)

_Cables de conexión y tipos para poder trabajar en CONSOLE:_
- Serial (RS232) a RJ45
- USB A a RJ45
- USB A a Mini USB (en dispositivos Cisco más modernos)

## Pasos a seguir
1. Reiniciar el router y presionar la secuencia de quiebre o _Break Sequence_ del terminal (`Ctrl + Break` o `Ctrl + Pause`)
	- El router/switch al encender ejecuta un proceso POST (Power On Self Test)
	- Luego pasa a un ROM Monitor, en este punto ejecutamos los comandos de recuperación
	- En una situación normal deberia pasar al bootloader el cual tiene instrucciones para buscar la IOS que esta almacenada en la flash generalmente. 
	- Una vez que el IOS haya arrancado correctamente, procede a buscar el archivo `starup-config` (almacenado en la NVRAM) con todas las configuraciones guardadas
		- En este archivo tambien se encuentran las contraseñas guardadas

	![](../_anexos_/Screenshot%20from%202024-01-02%2001-36-28.png)

2. Una vez que ROMMON inicie se debe indicar al router que se salte el archivo de configuración de arranque (`starup-config`) almacenado en la NVRAM
	- Esto se logra modificando el registro de configuración (confreg) al valor 0x2142 y reniciando.
		- El valor por defecto al iniciar el encendido es 0x2102
	
	![](../_anexos_/Screenshot%20from%202024-01-02%2001-43-27.png)

3. Al reiniciar, el sistema nos preguntará si deseamos la configuración inicial tal como si el router estuviese con los valores de fábrica. Le decimos que "no".

	![](../_anexos_/Screenshot%20from%202024-01-02%2001-49-42.png)

4. Una vez cargado el sistema, router estará en blanco. Ahora debemos ingresar al modo EXEC privilegiado y luego copiar la configuración de arranque (startup-config) hacia la RAM del router (running-config). Una vez cargada la configuración, cambiamos la contraseña de enable o las que necesitemos.
	- El proceso es inverso a `copy running-config startup-config`  
	 
	![](../_anexos_/Screenshot%20from%202024-01-02%2001-53-41.png)

5. El último paso es instruir al router que vuelva a leer la NVRAM en su próximo arranque, aplicando el comando `config-register 0x2102` y guardar la configuración activa en la RAM hacia la NVRAM.
 
	 ![](../_anexos_/Screenshot%20from%202024-01-02%2001-55-28.png)


