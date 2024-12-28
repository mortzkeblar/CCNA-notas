**Extended ACL** es una mejora sobre standard [[Project/Networking/CCNA-notas/Security/ACL/ACL|ACL]], ya que te permite realizar el control de tráfico en base a varios parametros (a diferencia del standard [[Project/Networking/CCNA-notas/Security/ACL/ACL|ACL]] que solo filtra por el _source IP address_).
- El protocolo del payload dentro del paquete ([[TCP]], [[UDP]], ICMP, etc)
- Source / Destination [[IP address]] 
- Source / Destination de puertos TCP / UDP 

La configuración es similar al standard ACL:
- Configuración de numbered ACL desde global config mode con `access-list`
- Configuración de named ACL desde named ACL config mode con `ip access-list`

### Matching protocol, source and destination 
Para configurar un extended numbered ACL, se usa la siguiente sintaxis. 
![[Pasted image 20241228002339.png]]

> Los _extended numbered [[Project/Networking/CCNA-notas/Security/ACL/ACL|ACL]]_ se deben encontrar en los rangos apropiados `[100, 199], [2000, 2699]`.

Para configurar un extended ACL en named ACL config mode, se usa la siguiente sintaxis.

![[Pasted image 20241228002540.png]]

 Los argumentos `protocol`, `source` y `destination` son los parametros que se agregan al ACE y que van a matchear con los paquetes sobre los que se desea realizar el control de tráfico.
 
 > La evaluación de los paquetes son contra todos los parámetros, no se realiza matching parcial.

![[Pasted image 20241228003048.png]]
- Los argumentos como `icmp` , `ospf`, `tcp`, etc se usan para hacer match con los paquetes que transporten payload de esos protocolos respectivamente 
- El argumento `ip` se puede usar en caso de que no le importe el protocolo de capa superior que esta transportando el paquete y solo se quiere basar en la dirección de origen / destino.
- `host <ip-addr>` permite especificar una única [[IP address]] que debe hacer match 

**Example ACEs for extended ACLs**
![[Pasted image 20241228015624.png]]

![[Pasted image 20241228022659.png]]

> En un standard [[Project/Networking/CCNA-notas/Security/ACL/ACL|ACL]], se recomienda aplicar el ACL lo más cercano posible del destino, debido a que si se aplica cerca al origen se puede bloquear trafico legitimo desde ese origen.
> En cambio en un **extended ACL**, al tener más control sobre el tráfico a filtrar es recomendable aplicar el mismo lo más cerca posible del origen.
> - Si se tiene el ACL configurado correctamente solo se bloquea el tráfico deseado 
> - Es más eficiente ya que el tráfico no tiene la necesidad de viajar por toda la red para terminar siendo bloqueado cerca del destino por el ACL 

### Matching TCP/UDP port numbers 
Además de las argumentos de protocolos a filtrar, tambien se puede especificar los puertos de [[TCP]] / [[UDP]] que puedan ser aceptados o denegados. 

![[Pasted image 20241228030202.png]]
Algunos ejemplos para los operadores a usar son:
- `eq 80` - igual al puerto 80 
- `gt 80` - mayor que 80 (no incluido)
- `lt 80` - menor que 80 (no incluido)
- `neq 80` - no es igual a 80 (cualquiera diferente de 80)
- `range 80 100` - desde 80 a 100 (incluyendo 80 y 100)

**Examples ACEs including port numbers**
![[Pasted image 20241228030625.png]]

En la siguiente  imagen se puede ver cada parte que conforma un ACE en un extended ACL, se puede observar que no es estrictamente necesario especificar tanto la puerto de origen y destino. 

![[Pasted image 20241228031405.png]]

![[Pasted image 20241228031432.png]]
Se puede observar que al tratar de ver los ACE configurados con `show access-list`, algunos protocolos (no todos) son nombrados por su nombre. Por ejemplo al configurar un ACE que involucre `UDP 123` este sera reemplazado por `ntp` que es el protocolo que funciona en ese puerto sobre UDP. 

![[Pasted image 20241228032905.png]]
Para el CCNA, puede ser de utilidad tener en cuenta los siguientes protocolos:
- TCP 20 (FTP data) - `ftp-data`
- TCP 21 (FTP control) - `ftp`
- TCP 23 (Telnet) - `telnet`
- TCP/UDP 53 (DNS) - `domain`
- UDP 67 (DHCP server) - `bootps`
- UDP 68 (DHCP client) - `bootpc`
- UDP 69 (TFTP) - `tftp` 
- TCP 80 (HTTP) - `www`

## Editing ACLs 
Poder editar ACLs es importante tanto para eliminar un ACE como para agregar entre medio de otros ACEs ya creados. 

En un _numbered ACL_ NO se puede eliminar ACEs individuales, para modificar un ACL se debe eliminar el ACL completo.

En un _named ACL_ es posible modificar los ACEs a conveniencia. Para borrar un ACE se puede usar `no <seq-num-ACE>` en el named ACL config mode

![[Pasted image 20241228034710.png]]
También se puede agregar un nuevo ACE especificando el número de secuencia, en el siguiente ejemplo se agrega un nuevo ACE en la posición `30` donde eliminamos el anterior ACE.

![[Pasted image 20241228034826.png]]

### Resequencing ACEs 
Named ACL config mode permite agregar nuevos ACEs en medio de los ACEs ya creados especificando el número de secuencia. 

Por defecto, el primer ACE creado tiene el valor de $10$ y cada nuevo ACE creado tiene un valor que es el incremento $+10$ del anterior ACE. 

Estos valores pueden ser modificados usando `ip access-list resequence { name-acl | number-acl } <starting-seq-num> <increment> `

![[Pasted image 20241228035446.png]]
Esto no solo permite modificar el valor para los nuevos ACEs, sino que tambien ajusta de forma automatica a los valores de secuencia de los ACEs que ya fueron creados. 

![[Pasted image 20241228035723.png]]
