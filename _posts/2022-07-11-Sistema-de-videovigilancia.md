---
layout: single
title: Linux - Sistema de Vigilancia 
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
  - Sistemas de Videovigilancia
  - Linux
  - MotionEye
tags:  
  - Seguridad
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

# MotionEyeOS

MotionEyeOS es una distribución de Linux de Sistema de Vı́deo Vigilancia, y se
basa en BuildRoot y utiliza Motion como backend y MotionEye como frontend.

## Preparando entorno de Trabajo

En mi caso, por preferencia utilizare MotionEye dentro de un contenedor Docker. Para eso deberé
preparar mi sistema , instalando Docker y Docker Compose.

## ¿Que es Docker?
Docker es una plataforma de contenerización de código abierto. Permite a los desarrolladores empaquetar
aplicaciones en contenedores. Los contenedores combinan el código fuente de la aplicación con las biblio-
tecas del sistema operativo y las dependencias necesarias para ejecutar el código en cualquier entorno.
Los contenedores simplifican la entrega de aplicaciones distribuidas.
Docker es esencialmente un kit de herramientas que permite a los desarrolladores crear, implementar,
ejecutar, actualizar y detener contenedores utilizando comandos a través de una única API.

## Funcionamiento de los contenedores

Los contenedores son posibles gracias al aislamiento de procesos y a las capacidades de virtualización
integradas en el kernel de Linux. Estas funcionan como grupos de control (Cgroups) para asignar recursos
entre procesos, y espacios de nombres para restringir un acceso de procesos o visibilidad a otros recursos
o áreas del sistema.
Como resultado, los contenedores ofrecen toda la funcionalidad y beneficios de las máquinas virtuales,
incluyendo aislamiento de aplicaciones, escalabilidad y capacidad de disposición. Sus ventajas son las
siguientes:

* Ligero: Los contenedores sólo incluyen los procesos y dependencias del sistema operativo necesarias
    para ejecutar el código.

* Mejor manejo de los recursos: Con contenedores se puede ejecutar varias veces las mismas
    copias de una aplicación en el mismo hardware.
    
* Mayor productividad de los desarrolladores: En comparación con las VM, los contenedores
    son más rápidos y fáciles de implementar, suministrar y reiniciar.

## Caracterı́sticas
    
* Portabilidad: Los contenedores LXC suelen hacer referencia a configuraciones especificas de la
    máquina, mientras que los contenedores Docker se ejecutan sin modificaciones en cualquier entorno
    3de escritorios, centro de datos y nube.

* Ligeros: Con los contenedores Docker solo se puede ejecutar un proceso en cada contenedor.
    Esto permite crear una aplicación que puede continuar ejecutándose mientras una de sus partes se
    desactiva.
    
* Creación automática de contenedores: Docker puede crear automáticamente un contenedor
    basado en el código de origen de la aplicación.
  
* Control de versiones: Docker puede rastrear versiones de una imagen de contenedor, retroceder
    a versiones anteriores y rastrear quién creó una versión.
    
* Reutilizar contenedores: Los contenedores existentes se pueden utilizar como imágenes base,
    esencialmente como plantillas para crear nuevos contenedores.
    
* Repositorio de contenedores: Los desarrolladores pueden acceder a un registro de código abierto
    que contiene miles de contenedores aportados por los usuarios.

## Herramientas de Docker

* DockerFile: Cada contenedor Docker se inicia con un archivo de texto simple que contiene ins-
    trucciones para crear la imagen del contenedor Docker. El DockerFile es una lista de instrucciones
    de interfaz de lı́nea de comandos que Docker Engine ejecutará para ensamblar la imagen.
    
* Imágenes Docker: Las imágenes Docker contienen el código de origen de la aplicación, herra-
    mientas, bibliotecas y dependencias que el código de la aplicación debe ejecutar como contenedor.
    Las imágenes Docker son archivos de sólo lectura.
    
* Contenedores Docker: Los contenedores Docker son las instancias activas en ejecución de imáge-
    nes Docker.
    
* Los contenedores son contenido en vivo, efı́mero y ejecutable. Se pueden interactuar con ellos
    utilizando comandos Docker.
    
* Docker Hub: Es el repositorio público de imágenes Docker. Las imágenes de contenedor provienen
    de proveedores de software comercial, proyectos de código abierto y desarrolladores individuales.
    Ademas de las imágenes que han sido producidas por Docker, Inc.
    
* Daemon Docker: El daemon Docker es un servicio que se ejecuta en el sistema operativo. Este
    servicio crea y gestiona las imágenes Docker.

## Instalación Docker

Antes de comenzar con la instalación es necesario que se instalen unas herramientas:
    
* apt-transport-https: permite que el administrador de paquetes transfiera datos a través de https.

* ca-certificates: permite que el navegador web y el sistema verifiquen los certificados de seguridad.
    
* curl: transfiere datos.
    
* software-properties-common: agrega scripts para administrar el software.

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
echo " deb [ arch = $ ( dpkg --print -architecture) signed-by=/etc/apt/keyrings/docker.gpg]https //download.docker.com/linux/debian$(lsb_release-cs)stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null

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

##Instalación Docker Compose

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

Dentro del directorio /srv/motioneye creare un documento llamado docker-compose.yaml y dentro del
mismo colocare el siguiente código, que me permitira crear y configurar el contenedor de MotionEye.


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

* Username: admin

* password:

