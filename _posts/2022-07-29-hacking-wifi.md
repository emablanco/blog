---
layout: single

title: Hacking Wifi - Pentesting Wifi

excerpt: "**El ataque que presento**
Consiste en obtener el handshake WPA/WPA2 que se lleva a cabo entre las 
estaciones (clientes Wi-Fi) y el BSSID (punto de acceso) para intercambiar 
la clave compartida de cifrado de la red Wi-Fi durante la fase de 
autenticación, y luego intentar romperlo off-line mediante un diccionario de 
contraseñas.
"
date: 2022-07-29

classes: wide

header:

    teaser: /assets/images/wifi/logo.png

    teaser_home_page: true
    
    icon: /assets/images/logofairy.png

categories:

    - Wifi Slax
    - Kali Linux
    - Hacking Wifi
    - Wifi
    - Pentesting

tags:  

- Hacking Wifi
---

![](/assets/images/wifi/wallpapers.jpg)

La idea de esta guía es para comprobar la seguridad de tu red wifi y no para
robar el WIFI a tu vecino.  

Para comenzar con esta practica se necesita una Distribución GNU/Linux, la suit
de software de seguridad inalámbrica **Aircrak-ng** y **MACCHANGER**.

* GNU/Linux: Recomiendo un live de Kali Linux o Wifi Slax.
* AIRCRACK-NG: Suite de software de seguridad inalámbrica. 
* MACCHANGER: Permite cambiar la Dirección **MAC** de una interfaz.

En caso de que se use alguna Distribución que no sea las orientadas a seguridad informatica
se puede instalar las herramientas de la siguiente manera:

```bash
apt install aircrak-ng macchanger
```
Una vez que identifiquemos el nombre de la interfaz wifi es necesario ponerla en
modo monitor.

**Modo Monitor:** El modo monitor nos permite escuchar y capturar los paquetes que viajan
en el aire, permitiendonos obtener la MAC de los dispositivos que se estan comunicando.

Hay que distinguir entre el **ESSID** que generalmente es el nombre que hace referencia
al la red wifi y el **BSSID** que hace referencia a la Dirección MAC del dispositivo.

Dentro de cada red **WIFI** se encuentran nodos conectados, de los podemos
obtener sus direcciones MAC las cuales estan asociada a la propia red.

##
















































