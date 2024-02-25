---
layout: single
title:  SERVIDOR DHCP
excerpt: "  "

date: 2024-02-25
classes: wide
header:
    teaser: ../assets/images/dhcp/logo.png
    teaser_home_page: true
    icon: ../assets/images/logofairy.png
categories:
    - Debian
    - dhcp
tags:
 - servidor
 - administrar red
---

![](../assets/images/dhcp/wallpapers.jpg)

## SERVIDOR DHCP - CONFIGURACIÓN 

- Editar el archivo __/etc/default/isc-dhcp-server__ 

```bash
INTERFACESv4="dhcp1 dhcp2" # las dos interfaces que usare para las redes
INTERFACESv6=""

```
[](../assets/images/dhcp/default_dhcp.png)

- Editar el archivo __/etc/default/isc-dhcp-server__

```bash
#configuraciones globales: para todas las subredes
option domain-name "ema.local";
option domain-name-servers 8.8.8.8, 8.8.4.4, 1.1.1.1, 1.0.0.1;

default-lease-time 600; #una ip se asigna por este tiempo 
max-lease-time 7200; #timepo maximo que se puede dar una ip 

authoritative;                 # este servidor sera el univo servidor dhcp de la red 

ddns-update-style none; #no usare dns para los host 
log-facility local1; # con rsyslog guardo los logo dentro de un archivo diferente a syslog

#tiene mayor importancia los que se definan en local
subnet 192.168.103.0 netmask 255.255.255.0 { #red1
    range 192.168.103.100 192.168.103.200;
    option subnet-mask 255.255.255.0; 
    option routers 192.168.103.1;
    option domain-name-servers 8.8.8.8, 8.8.4.4, 1.1.1.1, 1.0.0.1;
    option domain-name "lan1.local";
    default-lease-time 600;   
    max-lease-time 7200;     
}
subnet 192.168.104.0 netmask 255.255.255.0 { #red 2
    range 192.168.104.100 192.168.104.200;
    option routers 192.168.104.1;
    option domain-name-servers 8.8.8.8, 8.8.4.4, 1.1.1.1, 1.0.0.1;
    option domain-name "lan2.local";
    default-lease-time 600;    
    max-lease-time 7200;        
}
# este equipo cuando se conecte a la red lan recibira una ip statica
host pc1{
	hardware ethernet 52:54:00:f1:41:17;
	fixed-address 192.168.104.222;
}

```
[](../assets/images/dhcp/dhcpd.png)

- Editar el archivo __/etc/logrotate.d/dhcp__ 

```bash
/var/log/dhcp.log {
    rotate 7
    daily
    missingok 
    notifempty
    compress 
    delaycompress
    create 0644 root root  
}
```

 * rotate 7 : Solo se permiten 7 archivos de dhcp.log 
 * daily : La rotacion es diara.
 * missingok : No genera error si el archivo no existe.
 * notifempty : Si esta vacio no hace la rotacion.
 * compress : Los archivos rotados se comprimen .gz.
 * delaycompress : Solo comprime el archivo actual en la proxima rotacion.
 * create 0644 root root  : Crea el archivo con los siguientes permisos.


[](../assets/images/dhcp/rsyslog.png)

- Reiniciar los servicios:


```bash 
systemctl restart isc-dhcp-server.service 
systemctl restart rsyslog.service

```

- Ver las IP prestadas: 

```
cat /var/lib/dhcp/dhclient.enp1s0.leases

```

[](../assets/images/dhcp/servidor_ip.png)

### CLIENTE DHCP 

- Cliente DHCP __/etc/network/interfaces__

```bash
allow-hotplug enp1s0
iface enp1s0 inet dhcp

```
[](../assets/images/dhcp/clientes-red.png)



- Ver las IP que el **Servidor DHCP** me asigno:

```bash
cat /var/lib/dhcp/dhclient.enp1s0.leases

```



Con esto se logra tener un servidor DHCP funcional.




## QEMU/KVM


### SERVIDOR DE VIRTUALIZACION

- Crear interfaces virtuales

```bash

#! /bin/bash

echo "Creando red para servidor DHCP"
#interfaz 1
ip link add link br0 name dhcp1 type vlan id 103
ip addr add 192.168.103.1/24 brd 192.168.103.255 dev dhcp1
ip link set dhcp1 up

#interfaz 2
ip link add link br0 name dhcp2 type vlan id 104
ip addr add 192.168.104.1/24 brd 192.168.104.255 dev dhcp2
ip link set dhcp2 up

echo "Creacion de la interfaz DHCP finalizada"

```
- PERMITIR PING: 

```bash 
#borrar
/usr/sbin/iptables -A INPUT -i dhcp1 -p icmp --icmp-type echo-request -j ACCEPT #PING
/usr/sbin/iptables -A INPUT -i dhcp1 -p icmp --icmp-type echo-request -j ACCEPT #PING
/usr/sbin/iptables -A OUTPUT -o dhcp1 -p icmp --icmp-type echo-request -j ACCEPT #PING
/usr/sbin/iptables -A OUTPUT -o dhcp2 -p icmp --icmp-type echo-request -j ACCEPT #PING
/usr/sbin/iptables -A FORWARD -i dhcp1 -o br0 -p icmp --icmp-type echo-request -j ACCEPT #PING
/usr/sbin/iptables -A FORWARD -i dhcp2 -o br0 -p icmp --icmp-type echo-request -j ACCEPT #PING

```
- PERMITIR QUE DESDE EL FIREWALL ME PUEDA CONECTAR A LAS IP DETRAS DE LA INTEFAZ DHCP*

```bash 
/usr/sbin/iptables -A OUTPUT -o dhcp1 -p tcp --dport 2222 -j ACCEPT #SSH
/usr/sbin/iptables -A OUTPUT -o dhcp1 -p tcp --dport 22 -j ACCEPT #SSH
/usr/sbin/iptables -A OUTPUT -o dhcp2 -p tcp --dport 22 -j ACCEPT #SSH

```

- PUERTOS POR LOS QUE PODRAN SALIR LOS MIEMBROS DE LAS SUBRED 

```bash

#dhcp1
/usr/sbin/iptables -A FORWARD -i dhcp1 -o br0 -p tcp --dport 80 -j ACCEPT
/usr/sbin/iptables -A FORWARD -i dhcp1 -o br0 -p tcp --dport 443 -j ACCEPT
/usr/sbin/iptables -A FORWARD -i dhcp1 -o br0 -p tcp --dport 53 -j ACCEPT
/usr/sbin/iptables -A FORWARD -i dhcp1 -o br0 -p udp --dport 53 -j ACCEPT
#dhcp2
/usr/sbin/iptables -A FORWARD -i dhcp2 -o br0 -p tcp --dport 80 -j ACCEPT
/usr/sbin/iptables -A FORWARD -i dhcp2 -o br0 -p tcp --dport 443 -j ACCEPT
/usr/sbin/iptables -A FORWARD -i dhcp2 -o br0 -p tcp --dport 53 -j ACCEPT
/usr/sbin/iptables -A FORWARD -i dhcp2 -o br0 -p udp --dport 53 -j ACCEPT

```

- EL TRAFICO QUE SE ORIGINE EN LAS SIGUIENTES REDES SE ENMASCARA 

```bash
/usr/sbin/iptables -t nat -A POSTROUTING -s 192.168.100.0/24 -j MASQUERADE 
/usr/sbin/iptables -t nat -A POSTROUTING -s 192.168.101.0/24 -j MASQUERADE 
/usr/sbin/iptables -t nat -A POSTROUTING -s 192.168.102.0/24 -j MASQUERADE
/usr/sbin/iptables -t nat -A POSTROUTING -s 192.168.103.0/24 -j MASQUERADE
/usr/sbin/iptables -t nat -A POSTROUTING -s 192.168.104.0/24 -j MASQUERADE 

```


### CLIENTES

- Configurar la interfaz de red.

```bash 
#! /bin/bash

echo "Iniciando configuracion de las interfaces"
ip link add link enp1s0 name dhcp2 type vlan id 104 #enp1s0 pertene a la subred 104 (dhcp2)
ip link set dhcp2 up #levanto la interfaz
/usr/sbin/dhclient dhcp2 #solicito la ip y las demas configuraciones
ip addr del 192.168.0.101/24 dev enp1s0 #elimino ip estatica
ip route add default via 192.168.104.1 #defino el gateway
echo "Fin de la configuracion de las interfaces"

```

### SERVIDOR DHCP 


```bash
#! /bin/bash

echo "Configurando interfaz de red"

#RED 1
ip link set enp7s0 up

ip link add link enp7s0 name dhcp1 type vlan id 103

ip addr add 192.168.103.1/24 brd 192.168.103.255 dev dhcp1

ip link set dhcp1 up

#RED 2
ip link set enp8s0  up

ip link add link enp8s0 name dhcp2 type vlan id 104

ip addr add 192.168.104.1/24 brd 192.168.104.255 dev dhcp2

ip link set dhcp2 up

echo "Configuración Finalizada"

```
[](../assets/images/dhcp/server-dhcp.png)
