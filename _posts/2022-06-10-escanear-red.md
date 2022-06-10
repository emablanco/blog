---
layout: single

title: Escanear Red Local  

excerpt: "**ScanHost**

Herramientas echas en python y bash que permitiran detectar todos los host
conectados a la misma red local. Estas herramientas no hacen mucho ruido en la red
lo que nos evitaria que un IDS/IPS aplicara alguna politica sobre buestro host."

date: 2022-06-10

classes: wide

header:

    teaser: /assets/images/scanHost/bash-python.jpg

    teaser_home_page: true
    
    icon: /assets/images/logofairy.png

categories:

    - python3
    - bash

tags:  

- ScanHost
---

![](/assets/images/scanHost/python.jpg)

**¿Qué es el TTL?**

TTL significa «Time to Live» (tiempo de vida) y es un ajuste que determina el 
tiempo que el paquete es válidos y estara disponible en una red antes de que el 
router los borre.

## Script en BASH

Este script realizar un ping a toda la red, escluyendo la direccion de red y broadcast.

```bash

#! /bin/bash

#creo un bucle que va del 1 al 254
for i in $(seq 1 254);do
   
    #Una vez que ejecuto el binario ping redirijo la salida stdout y stderr a /dev/null
    timeout 1 ping -c 1 192.168.1.$i 2>/dev/null 1>/dev/null

    if [ $? -eq 0 ];then
       echo -e "\nHOST [*]" 
        python3 wichOS.py 192.168.1.$i
    fi
done

```
## Script en Python

```python

#! /bin/python3.9


#importo las librerias necesarias

#para usar expreciones regulares y buscar el ttl
import re

#para obtener la ip que ingrese el usuario por la consola o interprete
import sys

#para ejecutar procesos desde python . 
import subprocess

#solo se permiten dos argumentos, el nombre la herramienta y la IP

if len(sys.argv) != 2:

    print("[Uso] python3 {} <ip-address>".format(sys.argv[0]))

#termino la ejecucion.
    sys.exit(1)

def get_ttl(ip):

    #creo el proceso
    ping = subprocess.Popen(["/usr/bin/ping -c 1 {}".format(ip),""], \
            stdout = subprocess.PIPE, shell = True )

    #ejecutor el proceso llamado ping y lo almaceno en una tupla dentro de ttl
    ttl = ping.communicate()

    #busco dentro de la tupla el ttl y lo retorno como un numero entero
    ttl = int(re.findall(r'\d{1,3}',re.findall(r'ttl=\d{1,3}',\
            ttl[0].decode('utf-8'))[0])[0]) 

    return ttl 

def get_os(ttl):
    
#dependiendo del numero de ttl informara si es una maquina Windows o Linux
    if ttl > 0 and ttl <= 64:

        return "LINUX"

    elif ttl > 64 and ttl <= 128:
        return "WINDOWS"
    
    else:
        return "NOT FOUND"

if __name__ == '__main__':

    #la primera posicion es el nombre de la herramienta y el segundo es la ip
    ip = sys.argv[1]

    ttl = get_ttl(ip)

    os = get_os(ttl)

    print("ip => {}\nttl => {}\nos => {}".format(ip,ttl,os))
    
```


- [Repositorio](https://github.com/emablanco/detectar-host-en-la-red)

 
