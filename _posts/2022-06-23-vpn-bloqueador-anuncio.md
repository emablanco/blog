---
layout: single
title: VPN con Bloqueador de anuncios
excerpt: "**PIHOLE** es un bloqueador de anuncios que cubre toda la red y la protege de los anuncios y de rastreadores, evitando que se tenga que realizar configuraciones en cada dispositivos."

date: 2022-06-23
classes: wide
header:
  teaser: ../assets/images/vpn/logo-docker-wireguard-pihole.png
  teaser_home_page: true
  icon: ../assets/images/logofairy.png

categories:
  - Servidor VPN
tags:  
  - vpn
  - docker
  - pihole
  - linux
  - wireguard
  - docker-compose
---

## VPN

<p align="center">
<img src="/assets/images/vpn/vpn-seguridad.jpg">
</p>

## WIREGUARD

<p align="center">
<img src="/assets/images/vpn/wireguard.jpg">
</p>

WireGuard es una aplicación de software libre y que utiliza un protocolo de 
comunicación que implementa técnicas de red privada virtual (VPN)
para crear conexiones seguras punto a punto en configuraciones enrutadas o 
puenteadas. Se ejecuta como un módulo dentro del núcleo Linux y tiene como 
objetivo un mejor rendimiento que los protocolos de tunelización IPsec y 
OpenVPN. Fue escrito por Jason A. Donenfeld y se publica bajo la segunda 
versión de la GNU General Public License (GPL). 

- [Wikipedia](https://es.wikipedia.org/wiki/WireGuard)

## PIHOLE

<p align="center">
<img src="/assets/images/vpn/pi-hole_logo.png">
</p>


Pi-hole es una aplicación para bloqueo de anuncios y rastreadores en Internet
a nivel de red en Linux que actúa como un sumidero de DNS (y opcionalmente 
como un servidor DHCP), destinado para su uso en una red privada. Está diseñado 
para su uso en dispositivos embebidos con capacidad de red, como el Raspberry 
Pi, pero también se puede utilizar en otras máquinas que ejecuten distribuciones 
Linux e implementaciones en la nube.

Pi-hole tiene la capacidad de bloquear anuncios tradicionales de sitios web, 
así como anuncios en lugares no convencionales, como televisores inteligentes 
y sistemas operativos para dispositivos móviles.


- [Wikipedia](https://es.wikipedia.org/wiki/Pi-hole)

## DOCKER


<p align="center">
<img src="/assets/images/vpn/docker_container_engine_logo.png">
</p>


Docker es un proyecto de código abierto que automatiza el despliegue de 
aplicaciones dentro de contenedores de software, proporcionando una capa 
adicional de abstracción y automatización de virtualización de aplicaciones 
en múltiples sistemas operativos. 

- [Wikipedia](HTTPS://ES.WIKIPEDIA.ORG/WIKI/DOCKER_(SOFTWARE))


## DOCKER COMPOSE

<p align="center">
<img src="/assets/images/vpn/docker-wireguard-pihole.png">
</p>


```yaml

version: '3'
services:
  wireguard:
    image: linuxserver/wireguard
    container_name: wireguard
    cap_add: # Le damos permiso.. $man 7 capabilities
      - NET_ADMIN # permite configurar la red
      - SYS_MODULE # Carga modulo del kernel
    environment:
      - PUID=1000
      - PGID=1000
      - TZ=America/Argentina/Buenos_Aires
      - SERVERURL=mivpn.duckdns.org #dns o ip publica estatica
      - SERVERPORT=51820 #opcional puerto de wireguard, lo dejo por defecto
      - PEERS=2 #opcional cantidad de clientes a crear
      - PEERDNS=auto #opcional
      - INTERNAL_SUBNET=10.1.1.0 #opcional
    volumes:
      - /root/wireguard:/config
      - /lib/modules:/lib/modules
      - /usr/src:/usr/src
    ports:
        - 51820:51820/udp
    sysctls:
      - net.ipv4.conf.all.src_valid_mark=1 # permitir el forward
    dns:
      - 172.1.1.3 # el dns sera el servidor PIHOLE. Asi bloqueara los anuncios
    restart: unless-stopped #a no ser que el admin detenga el contenedor por cualquier cosa que se detenga, se iniciara de nuevo
    networks:
      containers:
        ipv4_address: 172.1.1.2 #ip que tendra el contenedor 
 
  pihole:
    container_name: pihole
    image: pihole/pihole:latest
    expose:
      - "53" #dns
      - "67" #para configurar la red
      - "80" #http
      - "443" #https
    environment:
      TZ: 'America/Argentina/Buenos_Aires'
      WEBPASSWORD: '123456'
    volumes:
      - './etc-pihole/:/etc/pihole/'
      - './etc-dnsmasq.d/:/etc/dnsmasq.d/'
    cap_add: #$man 7 capabilities
      - NET_ADMIN #podra realizar las configuraciones que se requieran en el contenedor
    restart: unless-stopped #simpre estara corriendo. a no ser que el admin lo detenga
    networks:
      containers:
        ipv4_address: 172.1.1.3 #ip del contendor
 
networks:
  containers:
    ipam:
      config:
        - subnet: 172.1.1.0/24 #red 

#https://firebog.net/ para agregar listas de bloqueo en pihole
```
