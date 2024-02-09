> route poisoning sets the hop count as unreachable on this RIP network 

Tambien llamada poison reverse, es una forma de [split horizon](Dynamic/split%20horizon.md)  que permite establecer la distancia de la red como infinita, provocando así que el resto de la red converga sin recibir actualizaciones imprecisas. Junto a [holddown timers](holddown%20timers.md)  puede ser una solución eficiente para evitar loops. 

Un ejemplo de esto que un router al ver que la red conectada directamente haya caido, envia un mensaje a los demás router indicando que ese red se encuentra a 16 hops de distancia. Viendo como funciona [RIPv2](RIPv2.md), esto es unreachable. 