# Proyecto Proxmox con nodos un Servidord de NFS y un Debian como router
## Recursos necesarios
Aqui dejare las paginas de descarga de las cosas mas importantes
A. [Proxmox] (https://www.proxmox.com/en/downloads)
B. [NFS-Server] (https://linuxconfig.org/how-to-set-up-a-nfs-server-on-debian-10-buster)
C. [Container-Template] (http://download.proxmox.com/images/system/)
## Indice
1. Que es proxmox
2. Instalacion del Proxmox
3. Crear Cluster Proxmox
4. Añadir nodos proxmox al cluster
5. Instalacion y configuracion de NFS
6. Añadir el NFS al Proxmox
7. Añadir Container al cluster

## ¿Que es proxmox?
Proxmox es una distribucion de virtualizacion que nos brinda las posibilidades de gestionar servidores virtuales todo al mismo tiempo.
### Instalacion de Proxmox
Hay que tener cuidado con una cosa antes de empezara instalar el proxmox y es el hostname ya que si se deja tal cual causara problemas mas adelante.
Por experiencia propia recomiendo poner algo asi:
```
pve1.exampleNode
```
Siempre un numero o una letra delante del pve si no causara conflictos entre los nodos y el propio proxmox.
Le he asignado al Proxmos original la IP 192.168.100.10 y como gateway que es la ip del Debian que actua como router es la 192.168.100.1
Una vez echo todo eso y la instalacion ya ha terminado pasamos a la creacion de la maquina del nodo.
La instalacion es la misma hay que tener cuidado con el hostname y asignarle una IP y la gateway del Router-Debian.
Y se comprueba que haya conexion entre el proxmox original y los nodos como el propio NFS

### Creacion de Cluster Proxmox
Para la creacion de un cluster bastara con poner el siguiente comando:
```
pvecm add NOMBRE-Cluster
```
Con esto ya esta la creacion del cluster de proxmox
Ahora pasamos a añadir los nodos al cluster.

### Añadir nodos al cluster proxmox

Una vez echo el cluster de proxmox hay que entrar desde el proxmox original al nodo que queramos añadir via SSH.
Una vez dentro pondremos el siguiente comando:
```
pvecm add IP-ADDRESS-Cluster
```
Como se puede ver la IP que tenemos que especificar es la del proxmox que contiene el cluster.
En caso de que falle le diremos que use SSH.
```
pvecm add IP-ADDRESS-CLUSTER --use_ssh
```
Y asi con cada nodo que queramos añadir al cluster.
Con el siguiente comando se comprueba de que los nodos estan añadidos al cluster:
```
pvecm nodes
```
Y nos tienen que aparecer con el hostname que le hemos asignado a ese nodo.

### Instalacion de NFS en debian
Para la instalacion del servicio de NFS ponemos el siguiente comando.
```
apt install nfs-kernel-server
```
Ya instalado vamos a hacer lo siguiente antes de tocar archivos de configuracion.
Creamos una carpeta en una directorio recomiendo que este en /home/NOMBRE-USUARIO para que asi no de problemas de permisos.
```
mkdir NFS-Shared /home/NOMBRE-USUARIO
```
Ahora vamos a ir directamente a editar el archivo exports que esta en la siguiente ruta
```
nano /etc/exports
```
Dentro al final del archivo pondremos lo siguiente:
```
/home/NOMBRE-USUARIO IP-RED-CON-PREFIJO(rw,sync,no_subtree_check)
```
Esto es lo que significa cada apartado que hay entre los parentesis
```
rw: operaciones de lectura y escritura.
sync: escribe y guarda en el disco antes de aplicarlo.
no_subtree_check: no mira si depente de alguien mas.
```
Ahora vamos a exportar el archivo compartido con el siguiente comando.

```
exportfs -r
```
### Añadir el NFS Server al cluster de proxmox.
Es una tarea muy facil, cogemos una maquina con entorno grafico el mismo servidor de NFS sirve.
Accedemos a traves del explorador web a nuestro Proxmox original con la ip que nos da.
```
https://IP:8006
´´´
Y desde alli vamos elegimos el cluster y vamos a la seccion de almacenamiento, y le damos a ADD y elegimos NFS.
Le decimos un ID para saber que es el NFS, la IP que tiene dicha maquina y donde se encuentra la carpeta que se va a compartir.
En la opcion de Content recomiendo elegir las 4 primeras por que mas adelante vamos a implementar los template para crear maquinas.
Se le da una IP de la red en la que estes con el gateway de la maquina que actue como router.
### Añadir Container al cluster.
Es una tarea algo sencilla lo unico que tendremos que hacer es descargar una plantilla y meterlo en la siguiente ruta dentro del Proxmox Original.
```
cd /mnt/pve/template/cache
```
Esto es para que cuando intentemos crear el contenedor salga la plantilla de la maquina que deseamos implementar.

### Configurar NAT Masquerade en la maquina Debian
Hay que hacer que pase todo el trafico por el router debian, sigamos los siguientes pasamos.
```
echo 1 > /proc/sys/net/ipv4/ip_forward
iptables -t nat -A POSTROUTING -s '192.168.100.0/24' -o TARJETA_RED -j Masquerade
```
### Crear el container
Para crear el container hay que acceder a traves del navegador al servidor principal de proxmox, y arriba del todo estara el boton.

Solo hay que seguir los pasos que nos indica y especificar donde queremos guardar el contenedor si bien en local o en uno de los nodos.
