---
tags:
  - IOS
  - CCNA
date created: Saturday, October 19th 2024, 11:08:14 pm
date modified: Wednesday, November 20th 2024, 11:33:50 pm
---

### IOS 12.3
- Una de las características principales es que para adquirir funcionalidades especiales (más allá de lo que ofrecía IP base). Se necesitaba cambiar el IOS completamente, en este gráfico se pueden observar los distintos IOS según funcionalidad, el precio también subía de acuerdo a los requerimientos.
![](Screenshot%20from%202024-01-02%2003-53-44.png)

### IOS 15
- La gran diferencia respecto a IOS 12 es que aca ahora tenemos una IOS universal
	- Esto significa que no necesitamos cambiar de IOS para poder obtener otras funcionalidades
- Para poder usar las funcionalidades avanzadas se necesita adquirir una licencia que te otorga permiso para poder usarlo. La cual se libera porque en realidad ya esta incorporada en la IOS. 
- Este sistema es valido para sistemas ISR G2 en adelante

![](Screenshot%20from%202024-01-02%2003-56-34.png)

### Unique Device Identifier (UDI)

![](Screenshot%20from%202024-01-02%2004-03-35.png)

``` bash
R1$ show license udi
```

#### Licenciamiento

![](Screenshot%20from%202024-01-02%2004-04-27.png)

#### Instalación de la licencia

![](Screenshot%20from%202024-01-02%2004-08-51.png)

``` bash 
Router(config)$ license install flash:archivo_de_licencia.xml
```