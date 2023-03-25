---
layout: single

title: Servidor Backup - GNU/Linux

excerpt: "**Rsnapshot**
crea Backup de manera automática a través de rsync, tanto en máquinas locales como remotas, a través de ssh. Rsnapshot utiliza enlaces duros para la realización de las copia, siempre que sea posible, ahorrando espacio en el disco.

La primera vez que se realiza una copia, se copian todos los datos, la segunda vez solo se copiaran aquellos ficheros nuevos, o los que hayan sido modificados.
"


date: 2022-08-07

classes: wide

header:

    teaser: ../assets/images/rsnapshot/logo.png

    teaser_home_page: true
    
    icon: ../assets/images/logofairy.png

categories:

    - Backup
    - Linux
    - ssh
    - rsync

tags:  

- Servidor Backup
---


![](/assets/images/rsnapshot//wallpapers.png)

Rsnapshot crea Backup de manera automática a través de rsync, tanto en máquinas locales como remotas, a través de ssh. Rsnapshot utiliza enlaces duros para la realización de las copia, siempre que sea posible, ahorrando espacio en el disco.

La copia completa solo se realiza la primera vez, la segunda vez solo se copiaran aquellos ficheros nuevos, o los que hayan sido modificados.

## Instalación 

Rsnapshot se encuentra disponible en los repositorios de **Debian** y para su instalación se debe ejecutar:

```bash
apt install rsnapshot
```

## Configuración 

Editar el fichero ubicado en _/etc/rsnapshot.conf_ y no se deben realizar espacio en el archivo, solo tabula.

### Valores importantes

* **snapshot_root** : Donde se van a guardar las copias de seguridad, **IMPORTANTE: LA BARRA AL FINAL SEÑALA QUE ES UN DIRECTORIO**

```bash
snapshot_root   /backup/
```

* **no_create_root:** indica si se crea automáticamente el directorio destino especificado en el valor anterior, rsnapshot_root, esto suele ser útil para unidades extraíbles

```bash
#no_create_root
```

* **cmd_cp:** ruta del comando cp (se deja como esta)

```bash
cmd_cp		/bin/cp
```
* **cmd_rm:** ruta del comando rm (se deja como esta)
* 
```bash
cmd_rm		/bin/rm
```
* **cmd_rsync:** ruta del comando rsync (se deja como esta)

```bash
cmd_rsync		/bin/bin/rsync
```
* **cmd_ssh:** ruta del comando ssh. descomentar si se va a utilizar

```bash
#cmd_ssh		/usr/bin/ssh
```
* **cmd_du:** si hemos instalado du, podemos saber el espacio que están ocupando las copias de seguridad. (se descomenta)

```bash
cmd_du		/usr/bin/du
```
* **cmd_rsnapshot_diff:** para ver las diferencias entre las diferentes copias de seguridad.

```bash
/usr/bin/rsnapshot-diff
```
* **retain:** indica la política deL copias: 
 * **daily 7:** haŕa una copia cada dia, manteniendo las 7 ultimas.
 * **weekly 4:** una copia cada semana, manteniendo las ultimas 4.
 * **mounthly 12:** una cada mes, manteniendo las 12 ultimas.
 * **yearly 5:** una copia cada año, manteniendo las ultimas 5 copias.

```bash
 retain daily 7
 retain weekly 4
 retain monthly 12
 retain yearly 5
```
* **logfile:** ubicación del archivo log de rsnapshot.

```bash
logfile	/var/log/rsnapshot.log
```
* **backup:** carpetas de las que queremos hacer copia y la ubicación donde se va a guardar. El parámetro exclude sirven para omitir carpetas o ficheros en la copia.

```bash
backup  natsu@192.168.1.4:/home/natsu/Documentos	natsu
backup  natsu@192.168.1.4:/home/natsu/Descargas	natsu	exclude=varios
```
El backup de este ejemplo lo realizare por **SSH** y para que funcione se debe crear la llave publica y privada y en el equipo cliente se debe instalar **rsync**.

### Generar clave Publica y Privada

```bash
ssh-keygen -b 4096
```

Enviar la clave publica al host remoto.

```bash
ssh-copy-id user@ip
```

## Comprobación 

Para comprobar si el fichero de configuración esta correcto se debe ejecutar.

```bash
rsnapshot configtest
Syntax OK
```
Se puede realizar una primera copia de seguridad a modo de control.

```bash
sudo rsnapshot -v daily
```

Ahora se puede ver que se a creado el directorio **daily.0/** y a su vez dentro de **daily.0/**, el directorio **natsu**, tal y como se indica en el fichero de configuración.

## ejecución automática

La automatización de las copias se realizara mediante cron.

```bash
nvim /etc/cron.d/rsnapshot
```

Aquí se debe definir el horario de las copias de seguridad.

Por defecto el archivo se encuentra con las siguientes configuraciones:

```bash
30 3    * * *           root    /usr/bin/rsnapshot daily
0  3    * * 1           root    /usr/bin/rsnapshot weekly
30 2    1 * *           root    /usr/bin/rsnapshot monthly
0  6    1 1 *           root    /usr/bin/rsnapshot yearly
```

Al Guardar las modificaciones ya estarán programadas las copias de seguridad.

El grueso de los datos siempre esta en **daily.0** que es la ultima copia realizada y los demás solo almacenan las diferencias. Pero si se navega por los directorios se verán todos los archivos, gracias a los enlaces duros.

## Controlar diferencias entre los backup

Para observar las diferencias se utilizar el comando **diff**.

```bash
rsnapshot -V diff daily.0 daily1
```

Con esto ya se tiene configurado un servidor de respaldo básico. También se puede especificar las configuraciones para que realice backup de las bases de datos.
