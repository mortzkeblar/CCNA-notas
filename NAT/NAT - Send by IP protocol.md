_Ver: [NAT - Inside and Outside](NAT%20-%20Inside%20and%20Outside.md)_

Esta sección consiste de una pequeña explicación de como funciona NAT a nivel del protocolo IP. 

![](../_anexos_/Screenshot%20from%202024-01-01%2009-46-54.png)

``` txt
Consideramos un paquete que esta en proceso en encapsular el contenido de la cabezera IP, asumimos que tiene la DATA de las capas superiores.
Recordemos que el IP header tiene: 
- IP source, para el ejemplo, PC1: 192.168.0.10
- IP destination, en este caso el destino es R2: 200.23.101.44 

Cuando el paquete llega al router, este se encarga de cambiar las dirección de origen y destino.
- IP src: 66.178.221.10 (IP del R1)
- IP dest: 200.23.101.44 (se mantiene)

IMPORTANTE: la IP src no solo cambia al de R1 porque la IP de PC1 solo funciona dentro de la LAN, tambien porque una vez llegue a R2 este tiene que devolver una respuesta en cuyo caso invierte las direcciones de origen y destino, entonces se garantiza que la respuesta devuelta llegue a PC1 a través de la IP publica de R1.

```