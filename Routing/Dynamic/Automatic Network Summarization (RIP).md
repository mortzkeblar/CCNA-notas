Esta opción generalmente viene activada por defecto, se puede comprobar a través de:
``` bash
R1$ show ip protocols
```

ANS significa que cuando el router tenga la información del enrutamiento, publica a sus vecinos el bloque classfull completo.
- Por ejemplo si un router tiene 3 IPs asignadas a tres interfaces, cuando envie la información a su vecino. En vez de enviar las tres IPs va a enviar una sola IP sumarizada en el limite de la clase (classfull).

Para deshabilitar ANS.
``` bash
R2(config-router)$ router rip
R2(config-router)$ no auto-summary
```