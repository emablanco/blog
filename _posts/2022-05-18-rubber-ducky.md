[---](---.md)
layout: single
title: Arduino - Rubber Ducky 
excerpt: "**¿Qué es USB Rubber Ducky?**
Casi todos los sistemas operativos de PC de Escritorios, portátiles, tablets y smartphones permiten la comunicación con el usuario a través de teclados USB. Es por eso que hay una especificación en el estándar USB conocida como HID (Human Interface Device). Esto permite que cualquier sistema operativo al se conecte el USB Rubber Ducky lo detectara y acepte automáticamente como si se hubiera conectado un teclado USB."
date: 2022-05-19
classes: wide
header:
  teaser: /assets/images/rubber/ducky.webp
  teaser_home_page: true
  icon: /assets/images/logofairy.png
categories:
  - Arduino
tags:  
  - bash
  - ino
---

![](/assets/images/rubber/ducky.webp)


**¿Qué es USB Rubber Ducky** Casi todos los sistemas operativos de PC de Escritorios o portátiles, y tablets o smartphones permiten la comunicación con el usuario a través de teclados USB. Es por eso que hay una especificación en el estándar USB conocida como HID (Human Interface Device). Esto permite que cualquier sistema operativo al se conecte el USB Rubber Ducky lo detectara y acepte automáticamente como si se hubiera conectado un teclado USB.


## Script Bash

```bash

#!/bin/bash

echo "Plug ARDUINO UNO into your PC with the two pins connected."

read

echo "Remove the jumper that connect pins."

read

echo "Reseting ARDUINO to default firmware..."

dfu-programmer atmega16u2 erase
dfu-programmer atmega16u2 flash --suppress-bootloader-mem Arduino-COMBINED-dfu-usbserial-atmega16u2-Uno-Rev3.hex
dfu-programmer atmega16u2 reset

echo "Unplug ARDUINO from pc and plug it again."

read

echo "Upload the code into ARDUINO"

read

echo "Connect two pins to reset ARDUINO"

read

echo "Remove the jumper that connect two pins"

read

echo "Writing Keyboard Firmware"

dfu-programmer atmega16u2 erase
dfu-programmer atmega16u2 flash Arduino-keyboard-0.3.hex
dfu-programmer atmega16u2 reset

echo "Done!"



```

Falta Terminal el Post... Cargar imagenes del proceso

- [ema](https://github.com/emablanco)
