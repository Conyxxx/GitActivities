# Crecion de DNS Primario y Subdominio con BIND 9
## Contenido
### 1. Instalacion de bind
### 2. Configuracion de DNS Maestro
___

#### 1. Instalacion de bind
Lo primero que hay que hacer es tener el sistema acutalizado,ya que pueden haber dependencias de librerias y al no estar actualizadas pueden causar problemas.

```
apt-get update
apt-get install bind9 bind9utils bind9-doc
```
Una vez ya instalado el bind 9 pasaremos a la creacion del DNS Maestro.
___

#### 2. Configuracion DNS Maestro
Para la creacion de un DNS Maestro hay que ir a la siguiente ruta y editar el siguiente archivo.
```
nano /etc/bind/named.conf.local
```
Y hay que ponerlo asi para la creacion del Servidor Maestro.


```
>Esto es la zona directa
zone "sri.iesiliberis.com" {
    type master;
    file "/etc/bind/db.sri.iesiliberis.com"; #Ruta del fichero de la zona directa.
    allow-transfer {192.168.100.0;};#Aqui va la ip de red no de host.
}
>Esto es la zona inversa
zone "0.168.192.in-addr.arpa"{
    type master;
    file "/etc/bind/db.192.168.0"; # Ruta del fichero para la zona inversa
}
```
Comprobamos que la sintasiz del named.conf
```
named-checkconf
```
!Atencion en caso de que no este bien leer el error suele ser muy intuitivo y te dice donde esta.

Ahora toca crear la zona apartir del archivo que nos da como ejemplo.

```
cp /etc/bind/db.local /etc/bind/db.sir.iesiliberis.com
```
Editamos y cambiamos el SOA, el NS.
```
$TTL    604800
@       IN      SOA     sri.iesiliberis.com. mario.sri.iesiliberis.com. (
                              3         ; Serial
                         604800         ; Refresh
                          86400         ; Retry
                        2419200         ; Expire
                         604800 )       ; Negative Cache TTL

; NS records
                        IN      NS      sri.iesiliberis.com.

; name servers - A records
sri.iesiliberis.com.      IN      A       192.168.0.1 # IP del servidor
```
Comprobamos que la sintaxis de la creacion de la zona directa este bien.

```
named-checkzone sri.iesiliberis.com /etc/bind/db.sri.iesiliberis.com
```
Ahora vamos a crear la zona inversa

Aqui hay que hacer lo mismo que con la zona directa pero en vez de copiar el db.local copiamos el db.127

```
cp /etc/bind/db.127 /etc/bind/db.192.168.0
```
Editamos el archivo y cambiamos los mismo parametros que en la zona directa.

```
$TTL    604800
@       IN      SOA     sri.iesiliberis.com. mario.srie.iesiliberis.com. (
                              3         ; Serial
                         604800         ; Refresh
                          86400         ; Retry
                        2419200         ; Expire
                         604800 )       ; Negative Cache TTL
;
; NS records
        IN      NS      mario.sri.iesiliberis.com.
```
Y para comprobar hay que hacer lo mismo que la zona directa.

```
named-checkzone 0.168.192.in-addr-arpa /etc/bind/db.192.168.0
```
Y para crear un DNS Esclado o un subdominio hay que hacer lo mismo solo hay que tener en cuenta cual es el maestro y el tipo que sera.