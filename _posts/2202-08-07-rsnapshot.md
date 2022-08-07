---
layout: single

title: Servidor Backup - GNU/Linux

excerpt: "**Backup** 
"


date: 2022-08-07

classes: wide

header:

    teaser: /assets/images/rsnapshot/logo.png

    teaser_home_page: true
    
    icon: /assets/images/logofairy.png

categories:

    - Backup
    - Linux
    - ssh
    - rsync

tags:  

- Servidor Backup
---


![](/assets/images/rsnapshot//wallpapers.png)

Rsnapshot crea Backup de manera automática a través de rsync, tanto en máquinas locales como remotas a través de ssh. Rsnapshot utiliza enlaces duros para la realización de la copia, siempre que sea posible, ahorrando espacio en el disco.
La primera vez que se realiza una copia, copias todas los datos, la segunda vez solamente copiara aquellos ficheros nuevos o que hayan sido modificados. En sus configuraciones se puede personalizar el intervalo de estas copias (hora, diario, semanal, mensual, anual), etc.

## Instalación 

Rsnapshot se encuentra en los repositorios de **Debian** por lo tanto con solo ejecutar el siguiente comando se instalara.

apt install rsnapshot

## Configuración 

Para realizar la configuración se debe editar el fichero ubicado en _/etc/rsnapshot.conf_

En el fichero de configuración se tabula.

Valores importantes #

Los valores a tener en cuenta en la configuración:

* **snapshot_root** : Donde se van a guardar las copias de seguridad, **importante** la barra final opara que reconozca el directorio
snapshot_root   /backup/

* **no_create_root:** indica si se crea automáticamente el directorio destino especificado en el valor anterior, rsnapshot_root, esto suele ser útil para unidades extraíbles

#no_create_root

* **cmd_cp:** ruta del comando cp (se deja como esta)

cmd_cp		/bin/cp

* **cmd_rm:** ruta del comando rm (se deja como esta)

cmd_rm		/bin/rm

* **cmd_rsync:** ruta del comando rsync (se deja como esta)

cmd_rsync		/bin/bin/rsync

* **cmd_ssh:** ruta del comando ssh. descomentar si se va a utilizar

#cmd_ssh		/usr/bin/ssh

* **cmd_du:** si hemos instalado du, podemos saber el espacio que están ocupando las copias de seguridad. (se descomenta)

cmd_du		/usr/bin/du

* **cmd_rsnapshot_diff:** para ver las diferencias entre las diferentes copias de seguridad.

/usr/bin/rsnapshot-diff

* **retain:** indica la politica de copias, daily 7 haŕa una copia cada dia, manteniendo las 7 ultimos dias, weekly 4 una cada semana , manteniendo las ultimas 4, mounthly 12 una cada mes, manteniendo las 12 ultimas, yearly 5 una copia cada año, manteniendo las ultimas 5 copias.

El nombre de los _retain_ pueden ser los que se quieran, en mi caso los llama de esta manera.

retain  dias   7
retain  semanas  4
retain  meses 12
retain  años  5

* **logfile:** ubicación del archivo log de rsnapshot

logfile	/var/log/rsnapshot.log

* **backup:** carpetas de las que queremos hacer copia y la ubicación donde se va a guardar la copia, el parámetro exclude sirven para omitir carpetas o ficheros en la copia.

backup  /home/ema/Escritorio           ema/

backup  /mnt/datos/       ema/  exclude=anime,exclude=series


Mi archivo de configuración

** Comprobación 

Para comprobar si el fichero de configuración de rsnapshot es correcto, ejecutamos


sudo rsnapshot configtest
Syntax OK

Podemos realizar una primera copia de seguridad con el comando

sudo rsnapshot dias


Podemos ver que se creado la carpeta dias.0/ y a su vez dentro de dias.0/, 
ha creado la carpeta datos tal y como se indica en el fichero de configuración,
las siguientes carpetas home/ema/Escritorio, mnt/datos.


