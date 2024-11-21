---
tags:
  - CCNA
date created: Monday, October 28th 2024, 1:23:48 am
date modified: Wednesday, November 20th 2024, 11:33:50 pm
---
Los cables de fibra optica, transmiten señales de luz a través de una fibra de vidrio. Si bien este "vidrio" es mucho mas flexible que el tradicional, aun así existe riesgo de romper la fibra muy facilmente. Incluso si se doble de forma muy pronunciada puede causar que la luz tenga fugas hacia fuera del cable, lo cual resulta en un debilitamiento de la señal.  

El cableado de fibra optica se prefiere sobre un [[Copper UTP]], por las limitaciones principalmente de tamaño maximo del cableado, que suele ser de 100 metros.

## The anatomy of a fiber-optic cable 
Una conexión de fibra optica normalmente consta de dos cables, uno para la transmisión y otra para la recepción de datos. Estos cables se conectan a un transceptor llamado _Small Form-Factor Pluggable (SPF)_ que se inserta en un puerto SFP dentro del dispositivo.

![[Pasted image 20241028024100.png|500]]
El cable de fibra optica esta formada por capas:
- Outer jacket / Buffer - sirven de protección y contensión a los componentes internos 
- Reflective cladding - ayuda a transportar la señal de luz a través del _glass core_
- Glass core - una capa de vidrio muy fina 

![[Pasted image 20241028024439.png|400]]

Dentro de los cables de fibra optica existen dos tipos. 
- Multimode fiber (MMF) - tiene un nucleo más ancho que un SMF, se usan en combinación con transmisores LED que envian la luz por el cable en multiples angulos (modes) reflectandode en el cladding
	- Los cables MMF generalmente soportan cientos de metros 
- Single-mode fiber (SMF) - usan un core mucho más angosto que combinado con transmisores laser envian la luz dentro del cable en un unico angulo
	- Los transmisores laser en general son mucho más caros que los transmisores LED usado en los MMF. 
	- Los cables SMF pueden soportar distancias de decenas de kilometros.

![[Pasted image 20241028024621.png|500]]

