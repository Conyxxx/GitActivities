---
title: "Configuracion del servicio de DHCP Relay en Mikrotik"
date : "05-10-2019"
output: "-"
---

### Introduccion
En el documento van a ver como se configura un DHCP Relay en Mikrotik

### Configuracion basica de IP y OSPF

___
Para esta practica hemos escogido un router de mikrotik y hemos accedido a el atraves de putty.
La IP por defecto es 192.168.88.1

```
Cuando se haya conseguido entrar dentro hay que ir a definir las interfacez de red.

Que es aplicando la siguiente sentencia:
```
ip interfaces address
```
Una vez que estamos aqui vamos a definirlas de la siguiente manera:
```
Ejemplo:

add address=192.168.2 netmask=255.255.255.0 interfaces=ether1
* En mi caso es la 192.168.0.8/29
    * Esa IP de red es la que tengo con mi compañero Maruan
* Y en el caso de mi compañero Chema es la siguiente:
    * 192.168.0.16/29
```
Eso se hace con cada interfaz de nuestro router
```OSPF```
Hay que poner la siguiente sentencia para poder acceder al routing del router.

Aqui hay que ir declarando las redes que tiene los brazos de nuestro router

* En mi caso mi router solo tenia 2 brazos
    * IP Red de maruan 192.168.0.8/29
    * IP Red de Chema 192.168.0.16/29

### Configuracion del DHCP Relay

Aqui ya lo unico que tenemos que tener en cuenta es quien va a ser el servidor de DHCP

Tenemos que tener cuidado con las interfaces por donde van a salir las direcciones

* Las sentencia a seguir es la siguiente:
    * add name=local interface=ether1 dhcp-server=192.168.0.17 local-address=192.168.0.18 disabled=no

Y asi por cada dhcp haya que declarar
