_Tomado de la clase 27, ACL estándar. [Link](https://youtu.be/nq1SoUM6XA0?list=PL2A7l6PiV52esSwosIAO86zf0RGe2pjTZ)_

> Nota: en la wildcard, un `0` significa que seleccionarán exactamente esos bits de la IP, mientras que un `1` significa que el bit en la IP puede cambiar, pero se seleccionará igual. 
### Example 1

![](_anexos_/Screenshot%20from%202023-12-28%2010-30-07.png)

![](_anexos_/Screenshot%20from%202023-12-28%2010-36-20.png)

### Example 2

![](_anexos_/Screenshot%20from%202023-12-28%2010-44-08.png)

### Example 3

![](_anexos_/Screenshot%20from%202023-12-28%2010-46-19.png)

Si contamos los bits de las wildcards, podemos sacar que tenemos $2^3$ combinaciones de IP excluidas, es decir `8`. 
- Desde `200.192.11.0` hasta `200.192.11.21` 
	- Tener en cuenta que no es todo el rango de IPs, sino los que coincidan en los bits de al wildcard. Es más facil ver eso sacando la cantidad de IPs excluidas, para saber que hay `8` y no `21`. 
	
![](_anexos_/Screenshot%20from%202023-12-28%2010-57-07.png)

### Exercise 1

![](_anexos_/Screenshot%20from%202023-12-28%2011-17-09.png)

### Exercise 2

![](_anexos_/Screenshot%20from%202023-12-28%2011-23-08%201.png)

En este ejercicio, no hay una solución directa ya que no podemos separar 13 hosts sin incluir a otras IPs que estan fuera del rango. 
- Además como se menciono antes, la cantidad de IPs son potencias de 2. Y no existe un número natural que cumpla $2^x=13$

![](_anexos_/Screenshot%20from%202023-12-28%2011-24-05%201.png)

