_Fixed-Lenght Subnet Masking_ (FLSM), es un metodo [[classless]] para la creación de subredes. Divide los bloques en subredes de igual tamaño. 

> En general, FLSM se considera un metodo legacy a favor del uso de [[VLSM]] debido a la eficiencia de este ultimo. 


## Subnetting /24 address blocks

La porción de red de un bloque de direcciones que se le asigna no se puede cambiar. Por lo cual el subnetting se realiza con los bits de la porción de host. 

Especifica se trata de tomar _prestados_ bits de la porción de host para agregarlos a la porción de red (y por ende el prefix lenght también se modifica).  Luego puede cambiar los valores de esos bits prestados para para crear diferentes subredes.

![[Pasted image 20241107022053.png|500]]
En este caso tenemos 2 subredes creados a partir del bit prestado. La formula general para saber la cantidad de subredes creadas es $2^x$ siendo $x$ la cantidad de bits prestados. 

![[Pasted image 20241107022524.png|500]]
El [[Subnetting]] para otros tipos de bloques de direcciones siguen la misma logica explicada anteriormente. 

## Subnettting /16 address blocks 
![[Pasted image 20241107033328.png|500]]

## Subnetting /8 address blocks 
![[Pasted image 20241107033451.png|500]]