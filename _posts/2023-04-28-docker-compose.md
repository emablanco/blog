---
layout: single

title: DOCKER - Servidor 

excerpt: "**DOCKER** es una plataforma de virtualización a nivel de sistema operativo que permite crear, ejecutar y distribuir aplicaciones en contenedores, lo que facilita el desarrollo, la implementación y la ejecución de aplicaciones en cualquier entorno."
date: 2023-04-28

classes: wide

header:

    teaser: ../assets/images/docker/logo.png

    teaser_home_page: true
    
    icon: ../assets/images/logofairy.png

categories:

    - servidor
    - docker
    - docker-compose

tags:  

- docker
---

![](../assets/images/docker/logo.png)

## docker

Docker es una plataforma de virtualización a nivel de sistema operativo que permite crear, ejecutar y distribuir aplicaciones en contenedores, lo que facilita el desarrollo, la implementación y la ejecución de aplicaciones en cualquier entorno.

Un contenedor es una unidad de software ligera que contiene todo lo necesario para ejecutar una aplicación, incluyendo el código, las bibliotecas y las dependencias, y se ejecuta de manera aislada del sistema operativo anfitrión y otros contenedores en la misma máquina. Docker utiliza una tecnología llamada Docker Engine para gestionar los contenedores y proporciona una API y herramientas de línea de comandos para crear, configurar y administrar los contenedores y las imágenes que los contienen.

La principal ventaja de Docker es que permite a los desarrolladores y los equipos de operaciones crear y ejecutar aplicaciones en cualquier entorno, ya sea en un servidor local, en una nube privada o pública, o en una infraestructura de contenedores orquestada como Kubernetes. Además, Docker simplifica la gestión de la infraestructura de la aplicación, ya que los contenedores son portátiles, escalables y se pueden mover fácilmente de un entorno a otro sin necesidad de preocuparse por las diferencias entre sistemas operativos o entornos de ejecución.

## Instalación

[DOCKER](https://docs.docker.com/)

<p style="text-align:center;">
 <img src="/assets/images/docker/docker.png">
</p>


Actualizar paquetes:

```bash
sudo apt-get update

``` 

Instalar paquetes necesarios para permitir que Debian 11 pueda usar paquetes a través de HTTPS:

``` 
sudo apt-get install -y apt-transport-https ca-certificates curl gnupg lsb-release

``` 
Agregar la clave GPG oficial de Docker al sistema:

``` 
curl -fsSL https://download.docker.com/linux/debian/gpg | sudo gpg --dearmor -o /usr/share/keyrings/docker-archive-keyring.gpg

``` 
Agregar el repositorio de Docker al archivo sources.list de Debian 11:

``` 
echo "deb [arch=amd64 signed-by=/usr/share/keyrings/docker-archive-keyring.gpg] https://download.docker.com/linux/debian $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null

``` 
Actualizar paquetes:

``` 
sudo apt-get update

``` 
Instala Docker Compose:

``` 
sudo apt-get install docker-compose

``` 
Docker Compose ahora debería estar instalado en Debian 11. Para verificar que la instalación se completo correctamente, ejecutar el comando:

``` 
docker-compose --version

``` 
Versión de Docker Compose que has instalado.
