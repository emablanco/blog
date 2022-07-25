---
layout: single

title: Nextcloud - Servidor QEMU/KVM 

excerpt: "**Nextcloud**
es un software bajo Licencia: AGPLv3, lo que significa que es Software Libre.
Este software permite crear un servicidor de alojamiento de archivos, para almacenar
todos nuestros documentos, videos, etc. De esta manera tenerlos disponibles desde cualquier lugar, y en todo momento.
Nextcloud cuanta con muchas funciones que lo hacen estar al mismo nivel que cualquier 
otro servicio de alojamiento de archivos en la nube.
"

date: 2022-07-22

classes: wide

header:

    teaser: /assets/images/nextcloud/logo1.png

    teaser_home_page: true
    
    icon: /assets/images/logofairy.png

categories:

    - Debian
    - Nextcloud
    - dmz


tags:  

- Nextcloud
---

![](/assets/images/nextcloud/nextcloud.webp)

## Instalación

Para comenzar con la instalacion de **Nextcloud** antes hay que resolver unas dependencias
las cuales se mencionan en su documentacion y descargar **Nextcloud Server**.

```bash
#Nextcloud Server
wget https://download.nextcloud.com/server/releases/latest.tar.bz2
#descomprimir tar
tar xjvf lates.tar.bz2
#mover el directorio nextcloud a opt
mv nextcloud /opt
#cambiar el dueño del directorio y los archivos que se encuentren dentro
chown -R www-data:www-data /opt/nextcloud/
```

- [Requisitos](https://docs.nextcloud.com/server/latest/admin_manual/installation/source_installation.html)

```bash
apt install php php-mysql php-mbstring php-json php7.4-common php7.4-xml php-zip php-gd curl php-curl php-pear php7.4-opcache php-intl mariadb-server

```
Luego que la instalación de los programas haya terminado es necesario realizar unas 
modificaciones en los valores predeterminados de **php** porque no seran 
utiles para **Nextcloud**.


```bash
#hacer backup del archivo original
cp /etc/php/7.4/cli/php.ini /etc/php/7.4/cli/php.ini.bk

nvim /etc/php/7.4/cli/php.ini
```
Dentro del documento es necesario que se cambien los siguientes valores:

* post_max_size = 511M
* upload_max_filesize = 511M
* memory_limit = 512M

Una vez que los cambios se realizaron se debe reiniciar el servicio de **apache2**

```bash
systemctl restart apache2

```
## MariaDB

Ahora se debe crear la base de datos que usara **Nextcloud** y realizar las 
configuraciones de seguridad de **MariaDB**.

Para configurar **MariaDB** se puede usar hacer uso de un script que viene junto
con la instalacion.

```bash
mysql_secure_installation
```
Finalizada la configuiración es hora de crear la base de datos y el usuario que la 
administrara.


```mariadb

MariaDB [(none)]> create database nextcloudb;
Query OK, 1 row affected (0.000 sec)

MariaDB [(none)]> CREATE USER 'nextcloud'@localhost IDENTIFIED BY '123456';
Query OK, 0 rows affected (0.020 sec)

MariaDB [(none)]> GRANT ALL privileges ON nextclouddb.* TO 'nextcloud'@localhost IDENTIFIED BY '123456';
Query OK, 0 rows affected (0.000 sec)

MariaDB [(none)]> flush privileges;
Query OK, 0 rows affected (0.000 sec)

MariaDB [(none)]> exit;
Bye

```

### HostVirtual

El archivo de HostVirtual se alojara dentro del directorio _/etc/apache2/sites-available/nextcloud.conf_
y tendra el siguiente contenido:

```bash
<VirtualHost *:80>
  DocumentRoot /opt/nextcloud/
  ServerName    nextcloud.duckdns.org 
  ServerAlias   nextcloud.duckdns.org
  <Directory /opt/nextcloud/>
    Require all granted
    AllowOverride All
    Options FollowSymLinks MultiViews

    <IfModule mod_dav.c>
      Dav off
    </IfModule>
  </Directory>
</VirtualHost>

```
Se pueden realizar otras configuraciones en el php para mejorar en funcionamiento
de **Nextcloud**.

```bash
nvim /etc/php/7.4/apache2/conf.d/10-opcache.ini

```
Dentro del ducumento se pueden agragar las siguientes opciones:

* zend_extension=opcache
* opcache.enable=1
* opcache.enable_cli=1
* opcache.memory_consumption=128
* opcache.interned_strings_buffer=8
* opcache.max_accelerated_files=10000
* opcache.revalidate_freq=1
* opcache.save_comments=1
* opcache.huge_code_pages=1

Una vez finalizadas las configuraciones anteriores es necesario
actvidar los modulos de **apache2** y reiniciar el servicio.

```bash

systemctl restart apache2
a2dissite 000-default.conf
a2ensite nextcloud.conf
a2enmod rewrite
a2enmod headers
a2enmod env
a2enmod dir
a2enmod mime

```

## Nextcloud

Finalizada la configuracion de puede acceder a **Nextcloud** y comezar a utilizarla sin problemas.
Como mi intención es poder usar este servicio desde internet tendre que crear el 
certificado **ssl**

![](/assets/images/nextcloud/nextcloud1.png)

## SSL

Para generar el sertificado ssl utilizare **CertBot**. Esta herramienta genera automaticamente los 
certificados y los actualiza.

Instalacion:

```bash

#Instalación
apt install certbot python3-certbot-apache
#configurar CertBot para apache
certbot --apache

```
Una vez generado el certificado para poder acceder desde internet al servidor **Nextcloud**
es necesario realizar una ultima configuracion:

Dentro del archivo _config.php_ se debe agregar el dns:

```bash

'trusted_domains' =>
  array (
   0 => 'localhost',
   1 => 'nextcloud.duckdns.org',
   2 => '192.168.1.222',
),

systemctl restart apache2

```



[CertBot](https://certbot.eff.org/instructions?ws=apache&os=debianbuster)
