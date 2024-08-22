Enhanced Interior Gateway Routing Protocol (EIGRP) es un [[dynamic routing protocols]] de tipo hibrido, es decir combina las caracteristicas de los protocolos basados en [[distance vector protocols]] y los [[link state protocols]]. 

EIGRP tiene varias ventajas como soporte VLSM, baja uso de CPU, se adapta bien en grandes entornos y tiene soporte para autenticación MD5. Tambien es el unico protocolo que soporta multiples protocolos de enrutamiento. 

Se le considera como un protocolo [[classless]], usa _Diffusing Update Algorithm (DUAL)_ para calcular las rutas loop-free en base a la información recopilada. Esta rastrea todas las rutas anunciadas por los routers vecinos y selecciona la mejor ruta basandose en metricas que consideran el ancho de banda, delay, confiabilidad y carga. 

