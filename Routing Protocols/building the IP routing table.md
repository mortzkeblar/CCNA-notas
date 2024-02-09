# building the IP routing table 
Recordad que la tabla de enrutamiento o Routing Information Base (RIB) contiene la información para poder reenviar el trafico (en caso de que las haya). La tabla por si mismo, no reenvía el trafico.

Los routers Cisco usan la distancia administrativa, la routing protocol metric y el prefix lenght para determinar cual ruta deberá ser agregada a la tabla de enrutamiento. 
- Si la ruta entrante no existe actualmente en la tabla, se añade a la tabla de enrutamiento.
- Si la ruta entrante es más especifica que otra ya existente en la tabla, se añade a la tabla. Notar que la ruta anterior aun permanece en la tabla.
- Si la ruta entrante es la misma que otra ya existente pero proviene de una ruta de origin preferente, esta reemplaza la viaja entrada.
- Si la ruta entrante es la misma que otra ya existente y ambos tiene el mismo protocolo:
	- Se descarta la nueva ruta si la [Metric](metric.md) es más alta que la ruta existente.
	- Reemplaza la ruta existente si la [metric](metric.md) de la nueva ruta es menor
	- Si la [metric](metric.md) para ambos es la misma, usa ambas rutas para load balancing (balanceo de carga)