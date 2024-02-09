# Toma de desición, el mejor camino
_Ver: [best routing path selection criteria](best%20routing%20path%20selection%20criteria.md)_

![](_anexos_/Screenshot%20from%202023-12-27%2016-57-08.png)

En este caso, el router debe tomar la decisión de por donde ir hacia 20.0.0.0, esto se define a través de la tabla de distancia. La mejor decisión es el valor más bajo, en este caso, EIGRP. 
- La `routing-table` solo contiene la información de la mejor ruta. Es decir, solo conserva lás mejores rutas a tomar y no todas las posibilidades de rutas (_Ver: [administrative distance](administrative%20distance.md)_).