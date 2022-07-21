---
layout: single

title: Configuración de red Servdior QEMU/KVM 

excerpt: "**La configuración de red**

consistira en crear tres interfaces virtuales (dmz,lan1,lan2) para gentionar los diferentes servicios."

date: 2022-07-20

classes: wide

header:

    teaser: /assets/images/debian.png

    teaser_home_page: true
    
    icon: /assets/images/logofairy.png

categories:

    - Debian
    - ip
    - iptables
    - vlan
    - dmz


tags:  

- Configuracón de Red
---

![](/assets/images/red/red.png)

## Introducción

Para la creacion de las interfaces virtuales usare la herramienta de red **ip**, con la cual 
creare las tres interfaces y sus configuraciones. Estas interfaces estaran asociadas a la 
interfaz virtual **br0** que funciona como puente con la interfaz fisica.

```bash
#! /bin/bash
echo "Creacion de las interfaces"

ip link add link br0 name dmz type vlan id 100
ip addr add 192.168.100.1/24 brd 192.168.100.255 dev dmz
ip link set dmz up

ip link add link br0 name lan1 type vlan id 101
ip addr add 192.168.101.1/24 brd 192.168.101.1 dev lan1
ip link set lan1 up

ip link add link br0 name lan2 type vlan id 102
ip addr add 192.168.102.1/24 brd 192.168.102.255 dev lan2
ip link set lan2 up

echo "Fin de las interfaces"
```
Este script de bash lo guardare dentro de un directorio y con **crontab** lo ejecutare cada vez
que inicie el sistema.

## Configuracón de iptables


```bash
#! /bin/bash

######################################################################################################
#SERVIDOR DE VIRTUALIZACION
#br0 => red local => 192.198.1.222
#dmz => vlan => dentro del servidor => 192.168.100.0/24
#lan1 => vlan => dentro del servidor => 192.168.101.0/24
#lan2 => vlan => dentro del servidor => 192.168.102.0/24

echo "Comienzo de las Reglas"

####################################################################################################
#VARIABLES
MAC_PC1="e8:03:9a:53:36:e9"
MAC_NET="b4:d5:bd:40:4d:27"
MAC_CEL="fc:a6:21:f9:0f:32"

####################################################################################################
#LIMPIAR REGLAS
/usr/sbin/iptables -F
/usr/sbin/iptables -X
/usr/sbin/iptables -Z
/usr/sbin/iptables -t nat -F

####################################################################################################
#POLITICAS POR DEFECTO
/usr/sbin/iptables -P INPUT DROP
/usr/sbin/iptables -P FORWARD DROP 
/usr/sbin/iptables -P OUTPUT DROP 

####################################################################################################
#LO QUE LLEGUE DE INTERNET
/usr/sbin/iptables -t nat -A PREROUTING -i br0 -p tcp --dport 22 -j DNAT --to 192.168.100.2:2222
/usr/sbin/iptables -t nat -A PREROUTING -i br0 -p tcp --dport 8096 -j DNAT --to 192.168.100.2:8096
/usr/sbin/iptables -t nat -A PREROUTING -i br0 -p tcp --dport 8080 -j DNAT --to 192.168.100.2:8080
/usr/sbin/iptables -t nat -A PREROUTING -i br0 -p tcp --dport 8765 -j DNAT --to 192.168.100.2:8765

####################################################################################################
#RESPUESTA A LAS COMUNICACIONES YA ESTABLECIDAS
/usr/sbin/iptables -A INPUT -m state --state RELATED,ESTABLISHED -j ACCEPT
/usr/sbin/iptables -A OUTPUT -m state --state RELATED,ESTABLISHED -j ACCEPT
/usr/sbin/iptables -A FORWARD -m state --state RELATED,ESTABLISHED -j ACCEPT

####################################################################################################
#PERMITEN LOS PING
/usr/sbin/iptables -A INPUT -i dmz -p icmp --icmp-type echo-request -j ACCEPT #PING
/usr/sbin/iptables -A INPUT -i lan1 -p icmp --icmp-type echo-request -j ACCEPT #PING
/usr/sbin/iptables -A INPUT -i lan2 -p icmp --icmp-type echo-request -j ACCEPT #PING
/usr/sbin/iptables -A OUTPUT -o dmz -p icmp --icmp-type echo-request -j ACCEPT #PING
/usr/sbin/iptables -A OUTPUT -o lan1 -p icmp --icmp-type echo-request -j ACCEPT #PING
/usr/sbin/iptables -A OUTPUT -o lan2 -p icmp --icmp-type echo-request -j ACCEPT #PING
#/usr/sbin/iptables -A FORWARD -i dmz -o br0 -p icmp --icmp-type echo-request -j ACCEPT 
#/usr/sbin/iptables -A FORWARD -i lan1 -o br0 -p icmp --icmp-type echo-request -j ACCEPT 
#/usr/sbin/iptables -A FORWARD -i lan2 -o br0 -p icmp --icmp-type echo-request -j ACCEPT 

####################################################################################################
#ADMINISTRAR FIREWALL DESDE MI RED LAN (NETBOOK,CELULAR,PC DESKTOP) 
/usr/sbin/iptables -A INPUT -i br0 -m mac --mac-source $MAC_CEL -p tcp --dport 2222 -j ACCEPT #CELULAR
/usr/sbin/iptables -A INPUT -i br0 -m mac --mac-source $MAC_PC1 -p tcp --dport 2222 -j ACCEPT #MI PC
/usr/sbin/iptables -A INPUT -i br0 -m mac --mac-source $MAC_PC1 -p tcp --dport 5900:5920 -j ACCEPT #SPICE
/usr/sbin/iptables -A INPUT -i br0 -m mac --mac-source $MAC_NET -p tcp --dport 2222 -j ACCEPT #NET
/usr/sbin/iptables -A INPUT -i br0 -m mac --mac-source $MAC_NET -p tcp --dport 5900:5920 -j ACCEPT #SPICE

####################################################################################################
#NFS => SOLO ES PRACTICA
/usr/sbin/iptables -A INPUT -s 192.168.100.2 -p tcp --dport 2049 -j ACCEPT #NFS
/usr/sbin/iptables -A INPUT -s 192.168.100.2 -p udp --dport 2049 -j ACCEPT #NFS

####################################################################################################
#CONEXION SSH DESDE EL FIREWALL A LAS DEMAS REDES
/usr/sbin/iptables -A OUTPUT -o dmz -p tcp --dport 2222 -j ACCEPT #SSH
/usr/sbin/iptables -A OUTPUT -o lan1 -p tcp --dport 2222 -j ACCEPT #SSH
/usr/sbin/iptables -A OUTPUT -o lan2 -p tcp --dport 2222 -j ACCEPT #SSH
/usr/sbin/iptables -A OUTPUT -o br0 -p tcp --dport 2222 -j ACCEPT #SSH

###################################################################################################
#PUERTOS QUE USA EL FIREWALL
/usr/sbin/iptables -A OUTPUT -o br0 -p tcp --dport 2222 -j ACCEPT #SSH
/usr/sbin/iptables -A OUTPUT -o br0 -p tcp --dport 80 -j ACCEPT #HTTP
/usr/sbin/iptables -A OUTPUT -o br0 -p tcp --dport 443 -j ACCEPT #HTTPS
/usr/sbin/iptables -A OUTPUT -o br0 -p tcp --dport 53 -j ACCEPT #DNS TCP
/usr/sbin/iptables -A OUTPUT -o br0 -p udp --dport 53 -j ACCEPT #DNS UDP

##################################################################################################
#PUERTOS POR LOS QUE LA DMZ PODRA SALIR
#dmz
/usr/sbin/iptables -A FORWARD -i dmz -o br0 -p tcp --dport 80 -j ACCEPT
/usr/sbin/iptables -A FORWARD -i dmz -o br0 -p tcp --dport 443 -j ACCEPT
/usr/sbin/iptables -A FORWARD -i dmz -o br0 -p tcp --dport 53 -j ACCEPT
/usr/sbin/iptables -A FORWARD -i dmz -o br0 -p udp --dport 53 -j ACCEPT
#lan1
/usr/sbin/iptables -A FORWARD -i lan1 -o br0 -p tcp --dport 80 -j ACCEPT
/usr/sbin/iptables -A FORWARD -i lan1 -o br0 -p tcp --dport 443 -j ACCEPT
/usr/sbin/iptables -A FORWARD -i lan1 -o br0 -p tcp --dport 53 -j ACCEPT
/usr/sbin/iptables -A FORWARD -i lan1 -o br0 -p udp --dport 53 -j ACCEPT
#lan2
/usr/sbin/iptables -A FORWARD -i lan2 -o br0 -p tcp --dport 80 -j ACCEPT
/usr/sbin/iptables -A FORWARD -i lan2 -o br0 -p tcp --dport 443 -j ACCEPT
/usr/sbin/iptables -A FORWARD -i lan2 -o br0 -p tcp --dport 53 -j ACCEPT
/usr/sbin/iptables -A FORWARD -i lan2 -o br0 -p udp --dport 53 -j ACCEPT

#################################################################################################
#LAS REDIRECCIONES QUE REALIZARA EL FIREWALL A LOS SERVIDORES
/usr/sbin/iptables -A FORWARD -i br0 -o dmz -p tcp --dport 8096 -j ACCEPT
/usr/sbin/iptables -A FORWARD -i br0 -o dmz -p tcp --dport 8080 -j ACCEPT
/usr/sbin/iptables -A FORWARD -i br0 -o dmz -p tcp --dport 8765 -j ACCEPT
/usr/sbin/iptables -A FORWARD -i br0 -o dmz -p tcp --dport 2222 -j ACCEPT

################################################################################################
#EL TRAFICO QUE SE ORIGINE EN LAS SIGUIENTES REDES SE ENMASCARA 
/usr/sbin/iptables -t nat -A POSTROUTING -s 192.168.100.0/24 -j MASQUERADE 
/usr/sbin/iptables -t nat -A POSTROUTING -s 192.168.101.0/24 -j MASQUERADE 
/usr/sbin/iptables -t nat -A POSTROUTING -s 192.168.102.0/24 -j MASQUERADE

```
- [Wikipedir](https://en.wikipedia.org/wiki/Libvirt)
- [KVM y QEMU](https://galvarado.com.mx/post/kvmqemu)
- [VT-x-Intel AMD-V](https://github.com/JJ/IV/blob/master/documentos/temas/Intro_concepto_y_soporte_fisico.md)
    
 
