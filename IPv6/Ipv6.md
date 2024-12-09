**IPv6 protocol** es la siguiente versión del protocolo [[IPv4 addressing|IPv4]], esta surge para superar las limitaciones de IPv4. 

Como una dirección IPv4 se compone de 32 bits, este tiene un tamaño máximo de $2^{32}=4,294,967,296$ direcciones IP, lo que en el principio de la era de internet parecía un limite muy lejano, en realidad resulta que ya llegamos al limite de direcciones disponibles. LACNIC por ejemplo fue el ultimo RIR en anunciar el agotamiento de sus direcciones IPv4 en 2020. 

> La organización encargada de la asignación de direcciones IP en el mundo es la llamada **Internet Asigned Numbers Authority (IANA)**, la IANA a su vez distribuye los espacios IP en otros organizaciones llamadas **Regional Internet Registries (RIR)**.
> ![[Pasted image 20241208222613.png]]


IPv6 esta compuesto de 128 bits, lo que nos da la cantidad de $2^{128}=340,282,366,920,938,463,463,374,607,431,768,211,456$ direcciones disponibles. 

Más alla de las diferencias de tamaño y formato, estos cumplen la misma función que IPv4, IPv6 encapsula segmentos [[Layer 4]] (i.e TCP o UDP) con un header para formar paquetes. Este proporciona direccionamiento end-to-end desde el origen del mensaje al destino. Así mismo, el paquete IPv6 es encapsulado en un frame [[Ethernet Header (and Trailer)|Ethernet]] en cada hop hacia su destino final. 

### Hex representation 
IPv6 se representa usando el sistema [[hexadecimal]], esto significa que cada digito hex contiene 4 bits de información: $2^{4}=16$, tambien llamada _nibble_. 

![[Pasted image 20241208223932.png]]

