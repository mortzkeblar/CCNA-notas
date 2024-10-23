---
tags:
  - CCNA
---
Desde la perpectiva del usuario, este se puede componer de 
- Nombre de usuario y contraseña
- Generadores de tokens
- Lectores de huellas digitales
- Combinación de multiples factores

Este proceso escala en complejidad ya que se debe usar varios tipos de autenticación a la vez que se debe asegurar que la autenticación se realice de forma segura sin comprometer las credenciales del usuario. 

Para proteger la información de autenticación se suele usar hashes, esta es una función criptográfica compleja que logra la traducción unidireccional de credenciales a algo llamado _message digest_. El digest es el resumen de información de entrada y se puede obtener mediante una serie de algoritmos. 
- MD5 (Message Digest algorithm 5)
- SHA (Secure Hash Algorithm)

![[Pasted image 20240624050311.png]]

### authentication methods 
- [[AAA]] 
- [[PKI]] 
- [[Kerberos]] 