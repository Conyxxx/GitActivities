# Conocer comandos de SYSTEMCTL,NETSTAT,SSH y Journalctl
### Contenido
1. **SYSTEMCTL**
2. **NETSTAT**
3. **SSH**
4. **JOURNALCTL**
<div style="text-align: justify">
En este documento hablare sobre las opciones mas conocidas y mas usadas de systemctl,netstat y el famoso SSH.

**SYSTEMCTL**
Systemctl es uno de los comandos mas utilizados en un servidore
ya que permite ver si un servicio esta en pleno funcionamiento
o por que pudo haber fallado.
* Comandos mas conocidos
```
-status "nombre servicio"
Verifica el estado de un servicio.
-restart "nombre servicio"
Reinicia el servicio.
```
**NETSTAT**
Es una herramienta a traves de comandos, se utiliza mucho para ver, conocer y identificar los puertos que usan los programas.
* Comandos mas usados
```
    - t: se usa para ver los procesos que usan el protocolo TCP
    - u: se usa para ver los procesos que usan el protocolo UDP
    - l: se utiliza para ver los purtos de escucha
    - p: se usa para ver los procesos.
    - n: se usa para ver la IP que tal proceso usa.
```
**SSH**

Cuando hablamos de SSH a todo el mundo nos viene a la cabeza las conexiones remotas.
Que pasaria si personas con mas conocimiento sobre este servicio se aprovecharian para entrar en servidores de empresas o incluso en nuestros ordenadores.
Acontinuacion mostrare algunos parametros muy importantes para asi poder vigilar mejor que no entren en las maquinas.

Configuracion

**Archivo de configuracion sshd_config**
Aqui podremos configurar todo con respecto a la conexion y quien se va a conectar.
El puerto por defecto de ssh es el 22, por ello hay que cambiarlo
```
nano /etc/ssh/sshd_config
buscamos la linea donde ponga "PORT" y se lo cambiamos por uno que este libre 
y no se solape con otro programa.
```
Otra cosa muy importante a tener en cuenta tambien es que usuarios se conectan a nuestro dispositivo.
Si queremos tener en cuenta quien es quien habria que hacer una lista blanca o WhiteList de los usuarios.
```
nano /etc/ssh/sshd_config
Aqui buscamos una linea que ponga AlloUsers, si no esta nos iremos hasta el final del 
archivo y escribimos lo siguiente.

AllowUsers user1 user2 user3
```
Ahora hay que configurar que root solo se pueda conectar con la clave instalada en el servidor.
Para ello hay que hacer los siguientes pasos.

Creamos una clave:
```
ssh-keygen -f clave
la opcion -f es para especificar el nombre de archivo.
cuando llega al apartado de la contraseña especificale una.
```
Una vez creado la clave ssh vamos a configurar el archivo sshd_config para decirle que haya autentificacion en el root.
```
nano /etc/ssh/sshd_config
buscamos las lineas donde ponga lo siguiente.
PasswordAuthentication yes
PermitROOTLogin yes
```
Se reinicia el server
```
service ssh restart
```
**Journalctl**
Es la herramienta que mas se usa para el ver los logs del sistema operativo
Comandos mas utiles de este servicio.
```
journalctl -r
Es para ver las entradas logs mas recientes
```

```
journalctl -n 3
Con la opcion -n se puede la cantidad de logs que hayas pasado
```
```
journalctl -p [crit],[debug],[info],[warning],[err],[alert],[emerg]
Eligiendo uno de los parametros podras ver todo servicio que tenga alomejor un error, o la informacion de algun servicio en concreto.
```
```
journalctl -u [systemd_unit]
Option que muestra las entradas log con una unidad especifica del sistema.
```
Ahora voy a explicar como exportar los logs de un servicio a formato json
```
journalctl -u [nombre servicio] -r -o json-pretty
Example:
journalctl -u ssh.service -r -o json-pretty
```
De esa manera nos mostrara los logs del servicio ssh en formato json.
Y si queremos guardarlo en un archivo con hacerle una doble redireccion.
</div>