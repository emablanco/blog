---
layout: single

title: Nube Personal - Servidor QEMU/KVM 

excerpt: "**Nextcloud**
es Software Libre y permite crear un servidor de alojamiento de archivos.
Esto ofrece la posibilidad de tener todos nuestros archivos centralizados y disponibles
en cualquier lugar, y en todo momento. Además, es multiplataforma y cuenta con 
aplicaciones clientes para todos los dispositivos. Esto permite el trabajo desde
un pc, celulares o tablets.

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

![](/assets/images/nextcloud/nextcloud3.png)

## Instalación

Descargar **Nextcloud**:

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
Para comenzar con la instalacion de **Nextcloud** antes hay que resolver unas dependencias
las cuales se mencionan en su documentacion.


- [Requisitos](https://docs.nextcloud.com/server/latest/admin_manual/installation/source_installation.html)

```bash
apt install php php-mysql php-mbstring php-json php7.4-common php7.4-xml php-zip php-gd curl php-curl php-pear php7.4-opcache php-intl mariadb-server

```
Una vez que la instalación de los programas finalizo es necesario realizar unas 
modificaciones en los valores predeterminados de **php**. 


```bash
#hacer backup del archivo original
cp /etc/php/7.4/cli/php.ini /etc/php/7.4/cli/php.ini.bk

nvim /etc/php/7.4/cli/php.ini
```
valores actualizados:
 
* post_max_size = 511M
* upload_max_filesize = 511M
* memory_limit = 512M

Reiniciar **apache2**

```bash
systemctl restart apache2

```
## MariaDB


Configuración de seguridad:

```bash
mysql_secure_installation
```
### Crear usuario y base de datos para Nextcloud

```mariadb

MariaDB [(none)]> create database nextclouddb;
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

### VirtualHost

Crar un archivo _nextcloud.conf_ dentro del directorio _/etc/apache2/sites-available/_
con el siguiente contenido:

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
Configuración adicional:

```bash
nvim /etc/php/7.4/apache2/conf.d/10-opcache.ini

```
* zend_extension=opcache
* opcache.enable=1
* opcache.enable_cli=1
* opcache.memory_consumption=128
* opcache.interned_strings_buffer=8
* opcache.max_accelerated_files=10000
* opcache.revalidate_freq=1
* opcache.save_comments=1
* opcache.huge_code_pages=1


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

![](/assets/images/nextcloud/nextcloud1.png)

## SSL

Generar  certificado **ssl** con **CertBot**.

```bash

#Instalación
apt install certbot python3-certbot-apache
#configurar CertBot para apache
certbot --apache

```

Dentro del archivo _/opt/nextcloud/config/config.php_ se debe agregar el dns:

```bash

'trusted_domains' =>
  array (
   0 => 'localhost',
   1 => 'nextcloud.duckdns.org',
   2 => '192.168.1.222',
),

```
Reiniciar servidor web:

```bash
systemctl restart apache2

```

![](/assets/images/nextcloud/nextcloud2.png)


[CertBot](https://certbot.eff.org/instructions?ws=apache&os=debianbuster)
