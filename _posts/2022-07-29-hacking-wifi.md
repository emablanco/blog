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


La idea de esta guia es para comprobar la seguridad de tu red wifi y no para
robar el WIFI a tu vecino.  

Para comenzar con esta practica se necesita una Distribución GNU/Linux, la suit
de software de seguridad inalámbrica **Aircrak-ng** y **MACCHANGER**.

* GNU/Linux: Recomiendo un live de Kali Linux o Wifi Slax.
* AIRCRACK-NG: Suite de software de seguridad inalámbrica. 

* MACCHANGER: Permite cambiar la Dirección **MAC**

En caso de que se use alguna Distribución que no sea las orientadas a seguridad informatica
se puede instalar las herramientas de la siguiente manera:

```bash
apt install aircrak-ng macchanger
```
![](/assets/images/wifi/logo.png)
