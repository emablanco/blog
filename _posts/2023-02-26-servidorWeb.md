---
layout: single
title: Servidor Web 
excerpt: "**SERVIDOR SEGURO**
Como esta practica esta orientada a colocar un servidor WEB (con acceso desde internet) en una red hogareña, es necesario proteger la red lan de cualquier intruso.
Dado que el servidor WEB sera accesible desde internet por cualquier persona, se puede dar el caso de que se lo logre comprometer, permitiendo al atacante tener acceso total a los demás HOST  de la RED LOCAL. Para proteger la red es necesario colocar el servidor detrás de una DMZ (zona desmilitarizada)."

date: 2022-05-19
classes: wide
header:
  teaser: ../assets/images/servidorWeb/logo.png
  teaser_home_page: true
  icon: ../assets/images/logofairy.png
categories:
  - web
tags:  
  - Firewall
  - web
---

### CREAR VLAN 

Para crear una VLAN se puede linkear la interfaz de red y crear una interfaz virtual que contendrá otro segmento de red.

Es necesario colocarle a la interfaz virtual un nombre que haga referencia al uso que se le pretende dar, para evitar confuciones en un futuro. 

- name: Le colocare de nombre VLAN (puede ser cualquier nombre).

- id: Identificador que hará referencia a la red. En este ejemplo el numero que le pondré es el 20. (Puede ser cualquiera numero).


```bash 

 ip link add link enp1s0 name vlan2 type id 20

```

### CONFIGURACIÓN IP

La red sera de tipo C y tendrá el siguiente segmento de red 192.168.1.0/24.

```bash 

#ip del servidor
ip addr add 192.168.20.2/24 dev vlan2

#habilitar interfaz
ip link set vlan2

#borrar la ip de la red lan
ip adr del 192.168.1.222/24 dev enp1s0 

#ip del router por defecto. sera la ip del firewall
ip route add default via 200.34.35.1 

#configurar dns
echo "nameserver 1.1.1.1" > /etc/resolv.conf

#apache como servidor web

apt instal apache

```

## FIREWALL

Finalizada la configuración del servidor WEB es necesario configurar el Firewall.


Del mismo modo que en el servidor WEB, se debe crear una interfaz virtual para conectarla a otro segmento de red. 

- br0: Este es el nombre de mi interfaz con la que cuento dentro del servidor de virtualización 

- name: El nombre puede ser cualquiera. Pero siempre se debe colocar uno que haga referencia al uso que se le pretende dar.

- id: El identidicador debe ser el mismo que el que se coloco en la interfaz virtual del servidor WEB, para que se conecte al mismo segmento de red.


```bash 

 ip link add link br0 name vlan2 type id 20

```

### CONFIGURACIÓN DE LA INTERFAZ VIRTUAL

Dentro del firewall se debe configurar la interfaz virtual y las reglas del Firewall.

```bash 

ip addr add 192.168.20.1/24 dev vlan2

ip link set vlan2

#todo lo que salga de la red 192.168.20.0/24 se enmascara
iptables -t nat -A POSTROUTING -s 192.168.20.0/24 -j MASQUERADE

#todo lo que se origine dentro de la red 192.168.20.0/24 se rechaza.
iptables -t -A FORWARD -s 192.168.20.0/24 -d 192.168.1.0/24 -j DROP

#se verifica que si se intenta estableces una cominidacion por la interfaz "br0" por el puerto 22,80, con protocolo TCP, se le aplique la politica de ruteo y se redirija al puerto correspondiente del servidor WEB.

iptables -t nat -A PREROUTING -i br0 -p tcp --dport 22 -j DNAT --to 200.34.35.10:22
iptables -t nat -A PREROUTING -i br0 -p tcp --dport 80 -j DNAT --to 200.34.35.10:80

```

Para que el servidor Firewall pueda redirigir los paquetes de una interfaz a otra interfaz se debe habilitar el ip forwarding


```bash

vi /etc/sysctl.conf

```

* NOTA:

La politica por defecto del Firewall deberia ser DROP. Esto requiere un conocimiento extra del los puertos que se deben mantener abiertos,etc. 




