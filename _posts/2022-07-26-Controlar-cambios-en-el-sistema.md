---
layout: single

title:  Controlar cambios en tu servidor

excerpt: "**Scripts**
en bash para descubrir cambios en los archivos del sistema y sabiendo esto 
tomar decisiones sobre los archivos alterados."

date: 2022-07-26

classes: wide

header:

    teaser: /assets/images/cambios-so/logo.png

    teaser_home_page: true
    
    icon: /assets/images/logofairy.png

categories:

    - Debian
    - Seguridad
    - md5sum
    - Hash


tags:  

- md5sum
---

![](/assets/images/cambios-so/md5sum.png)

## Generar Firmas de archivos


```bash

workdir=$PWD/seguridad
 
echo "Crear Base de datos del HASH de cada archivo"
 
echo ""
 
find /usr -type f -exec md5sum {} \; > $workdir/usr.txt
find /boot -type f -exec md5sum {} \; > $workdir/boot.txt
find /opt -type f -exec md5sum {} \; > $workdir/opt.txt
find /etc -type f -exec md5sum {} \; > $workdir/etc.txt
find /var -type f -not -path "/var/pool/*" -not -path "/var/log/*" -not -path "/var/tmp/*" -exec md5sum {} \; > $workdir/var.txt
 
echo ""
 
echo "Fin..."

```
## Comparar las firmas de los archivos.

```bash
#! /bin/bash
 
#crear directorio llamado "seguridad"
 
workdir=$PWD/seguridad
 
echo "Crear Base de datos del HASH de cada archivo"
 
echo ""
 
find /usr -type f -exec md5sum {} \; > $workdir/usr.tmp
find /boot -type f -exec md5sum {} \; > $workdir/boot.tmp
find /opt -type f -exec md5sum {} \; > $workdir/opt.tmp
find /etc -type f -exec md5sum {} \; > $workdir/etc.tmp
find /var -type f -not -path "/var/pool/*" -not -path "/var/log/*" -not -path "/var/tmp/*" -exec md5sum {} \; > $workdir/var.tmp
 
echo ""
 
echo "Diferencias..."
 
diff $workdir/usr.txt $workdir/usr.tmp
diff $workdir/boot.txt $workdir/boot.tmp
diff $workdir/opt.txt $workdir/opt.tmp
diff $workdir/etc.txt $workdir/etc.tmp
diff $workdir/var.txt $workdir/var.tmp
 
echo ""
echo "Limpiar"
 
rm -f $workdir/usr.tmp $workdir/boot.tmp $workdir/opt.tmp $workdir/etc.tmp $workdir/etc.tmp $workdir/var.tmp

```
