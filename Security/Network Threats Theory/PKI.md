---
tags:
  - CCNA
date created: Saturday, October 19th 2024, 11:08:14 pm
date modified: Wednesday, November 20th 2024, 11:33:50 pm
---
Public Key Infrastructure (PKI) es una técnica de autenticación compleja que funciona mediante certificados digitales. Estos proporciona un cierto nivel de confianza a la comunicación ya que autentican al remitente. PKI utiliza el concepto de _Certificate Authority (CA)_ para la entidad central que emite certs a los usuarios. CA confirma la identidad de cada usuario y todos los usuarios de una org confían en la CA.

Otro enfoque no centralizado es _web of trust_, los usuarios firman certificados para personas que conocen y esas personas firman certificados para otras personas que conocen, formando así una red de confianza. 

Las PKI están diseñadas para gestionar certificados, incluido la emisión, asignación y verificación. Estos puede funcionar bajo estos dos tipos de cifrado.
- Cifrado simétrico, este implica el uso de la misma clave para el cifrado y descifrado de datos
- Cifrado asimétrico, proporciona al usuario una clave pública utilizada para cifrar los datos, los cuales se envían y se descifran con una clave diferente llama clave privada, la cual se crea junto con la clave pública y estan relacionadas de forma criptograficamente entre sí. La clave pública se otorga a los usuarios para que puedan cifrar los datos que solo se pueden descifrar utilizando la clave privada asociada que solo conoce la entidad generadora de claves. 