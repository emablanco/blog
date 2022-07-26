---
layout: single
title: Sistema de Vigilancia Casero.
excerpt: "**¿Qué es un Sistema de Videovigilancia?**

Los Sistemas de Vigilancia permiten comprobar desde otra ubicación, el funcionamiento o el estado de un lugar"
date: 2022-07-11
classes: wide
header:
  teaser: /assets/images/motioneye/logo.png
  teaser_home_page: true
  icon: /assets/images/logofairy.png
categories:
  - Seguridad
  - Sistemas de Video Vigilancia
  - Linux
  - MotionEye
tags:  
  - Camaras
  - Videovigilancia
---
<p align="center">
<img src="/assets/images/motioneye/1.png">
</p>

## Motion 

Motion es un programa que monitorear la señal de vı́deo de una o varias cámaras
y puede detectar si una parte de la imagen ha cambiado; lo que se entiende como detectar movimiento.

El programa está escrito en C y está hecho para ejecutarse en Linux, desde
lı́nea de comandos, y su salida puede ser secuencias de vı́deo jpeg, mpeg, etc. Motios corre en segundo plano
,como un daemon, sin consumir mucho recurso del sistema.

## MotionEye

Para utilizar Motion de una manera mas amigable se encuentra disponible MotionEye, que  es u frontend para Motion.

## MotionEyeOS

MotionEyeOS es una distribución de Linux de Sistema de Vı́deo Vigilancia, y se
basa en BuildRoot y utiliza Motion como backend y MotionEye como frontend.

## Preparando entorno de Trabajo

En mi caso, por preferencia utilizare MotionEye dentro de un contenedor Docker. Para eso deberé
preparar mi sistema instalando Docker y Docker Compose.

## ¿Que es Docker?
Docker es una plataforma de contenerización de código abierto. Permite a los desarrolladores empaquetar
aplicaciones en contenedores. Los contenedores combinan el código fuente de la aplicación con las biblio-
tecas del sistema operativo y las dependencias necesarias para ejecutar el código en cualquier entorno.
Los contenedores simplifican la entrega de aplicaciones distribuidas.
Docker es esencialmente un kit de herramientas que permite a los desarrolladores crear, implementar,
ejecutar, actualizar y detener contenedores utilizando comandos a través de una única API.

## Instalación Docker

```bash
# actualizar lista de paquetes
apt update
# instalar herramientas
apt install curl apt - transport - https ca - certificates software - properties - common

#Clave GPG
# crear directorio
mkdir -p /etc/apt/keyrings
# descargar key
curl - fsSL https://download.docker.com/linux/debian/gpg | gpg --dearmor -o /etc/apt/keyrings/docker.gpg

#Agrega el repositorio:
# agregar repositorio
echo " deb [arch = $(dpkg --print -architecture) signed-by=/etc/apt/keyrings/docker.gpg]https //download.docker.com/linux/debian$(lsb_release-cs)stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null

#Instalar Docker Engine.
# actualizar lista de paquetes
apt update
# instalar docker
apt install docker-ce docker-ce-clicontainerd.io docker-compose-plugins

#Para verificar que docker se esta ejecutando correctamente ejecutare el contenedor de Hello-World, el
#cual da una bienvenida.
# descargar y ejecutar contenedor de bienvenida
docker run hello-world

```

## ¿Que es Docker Compose?

Docker Compose es una herramienta que permite simplificar el uso de Docker a partir de un archivo
YAML. De esta manera es mas sencillo crear contendores, conectarlos, habilitar puertos, volúmenes, etc.

## Instalación Docker Compose

Hay varias versiones de Docker Compose, aunque siempre es preferible la versión estable.

```sh
# descargar binario de docker - compose
curl -L " https://github.com/docker/compose/releases/download/1.26.0/docker-compose-$( uname -s )-$( uname -m )" -o /usr/local/bin/docker-compose
# cambiar permisos
chmod + x / usr / local/bin/docker-compose
# controlar la version de docker - compose
docker-compose --version

```
## Contenedor MotionEye

La instalación de MotionEye para el acceso local es muy sencilla 
cuando se utilizan Docker.Dentro del directorio /srv/motioneye creare un documento llamado docker-compose.yaml 
que contendra el siguiente código.


```yaml
version: "3"
services:
    motioneye:
        image: ccrisan/motioneye:master-amd64 #funciona para grabar
        #image: ccrisan/motioneye:dev-amd64 #me deja grabar, solo toma fotografias
        container_name: motioneye
        volumes:
            - /etc/localtime:/etc/localtime:ro #configuracion de la zona horaria
            - ./configuracion:/etc/motioneye #Lugar de almacenamiento
            - ./archivos:/var/lib/motioneye #Archivos de configuración
        ports:
            - 8765:8765 #puerto de
```

## Las credenciales predeterminadas son:

Una vez que se haya implementado, para acceder a **Motioneye** 
http://your-server-address:8765. Las credenciales predeterminadas son:

![](/assets/images/motioneye/motioneye.png)

* Username: admin

* password:


No hay contraseña. Simplemente se debe ingresar el nombre de usuario
"admin" y hacer clic en el botón de inicio de sesión.

**NOTA:** Asegúrese de cambiar la contraseña de las dos cuentas de usuario 
que están configuradas de manera predeterminada. Una vez que se haya iniciado 
sesión, se puede agregar una cámara haciendo clic en el menú con forma de
hamburguesas en la parte superior izquierda y luego haciendo clic en el menú 
desplegable que aparece. Para agregar cámaras IP, se deberá conocer la 
dirección IP de cada cámara. En esta demostración uso la cámara de mi celular
con **DroidCam**. La dirección proporcionada por **DroidCam** es mi caso es:

<p align="center">                                                
 <img src="/assets/images/motioneye/droidcam.jpg" width="300" height="300">
</p>

## Agregar Camara IP

![](/assets/images/motioneye/agregar-camara.png)

## Cliente Android

Motioneye tambien cuenta con un cliente para Android.

<p align="center">
<img src="/assets/images/motioneye/motioneye-android.jpg" width="300" height="300">
</p>

