# Conocer comandos de SYSTEMCTL y NETSTAT 
### Contenido
1. **SYSTEMCTL**
2. **NETSTAT**
3. **SSH**
4. **JOURNALCTL**

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

**NETSTAT**
Es una herramienta a traves de comandos, se utiliza mucho para ver, conocer y identificar los puertos que usan los programas.
* Comandos mas usados
```
    - t: se usa para ver los procesos que usan el protocolo TCP
    - u: se usa para ver los procesos que usan el protocolo UDP
    - l: se utiliza para ver los purtos de escucha
    - p: se usa para ver los procesos.
    - n: se usa para ver la IP que tal proceso usa.

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
**Journalctl**
Es la herramienta que mas se usa para el ver los logs del sistema operativo