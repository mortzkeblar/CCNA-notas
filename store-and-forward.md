Este método lee el frame completo, lo copia dentro de un buffer. Realiza una _cyclic redundancy check (CRC)_, y reenvía el frame si pasa la verificación. 

CRC verifica que no haya ningún error en el frame, el switch también realiza el check de que el frame este entre 64 y 1518 bytes. Cualquier frame fuera de ese rango se descarta automáticamente, además de los frames con errores. 

Este método de forwarding es el que tiene la latencia más alta, pero a su vez es de los métodos mas confiables. 

Esta método se encuentra por defecto para los Cisco 2960, estos ofrecen store-and-forward basado en hardware que funcionan como _wire speed_ y ofrece una latencia minima, lo cual lo hace equiparable al metodo [[cut-through]].



### related 
- [[cut-through]]  
- [[fragment-free]] 