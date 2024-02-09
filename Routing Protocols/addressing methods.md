Tanto [classful](classful.md) como [CIDR](CIDR.md) se refieren al asignamiento IP. 
- [classful](classful.md) addressing is IANA/RIRs assigning IP space from Class A, B, or C blocks (_legacy_).
- [classless](classless.md) ([CIDR](CIDR.md))  is IANA/RIRs assigning IP space in any size block, as required (_modern standard_).

[VLSM](../VLSM.md) y FLSM, hacen referencia a la forma en la que deberia asignar subredes dentro de su infraestructura (p. ej. si el protocolo de enrutamiento envia una mascara de subred). 
- [VLSM](../VLSM.md) allows any IP subnet within your deployment to be _any_ size (_modern standard_).
- **[FLSM](https://www.practicalnetworking.net/stand-alone/classful-cidr-flsm-vlsm/#flsm)** mandates that every IP subnet within your deployment be the _same_ size (_legacy_).