

_Ver: [TTL](TTL.md) _
Esto es una función de [RIPv2](RIPv2.md)   el cual te limita la cantidad de saltos a 15. Si un paquete llega a superar los 15 hops, este se descarta. 
Esto tiene un inconveniente y es que solo podemos usar RIP en redes que tengan como maximo, 15 saltos de distancia entre cada router.

Esto soluciona el [counting to infinity](counting%20to%20infinity.md)  pero no los [routing loops](routing%20loops.md) , porque siempre puede volver a comenzar de nuevo.
