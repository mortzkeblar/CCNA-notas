---
tags:
  - SW_stacking
  
---

# Elección de roles

Para el que el switch stacking funcione necesitamos definir un switch _master_ y otros switch en mode _slave_. 
![](_anexos_/Screenshot%20from%202024-01-05%2007-30-53.png)

1. El switch que actualmente es el master del stack
2. El switch con el valor de prioridad mas alto
3. El switch que no use la configuracion de interfaz por defecto
4. El switch con la prioridad de HW/SW mayor en el siguiente orden
	1. IP services con criptografia (K9)
	2. IP services sin criptografia
	3. IP base con criptografia (K9)
	4. IP base sin criptografia
5. El switch que enciende primero (uptime)
6. El switch con la MAC mas baja