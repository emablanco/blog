---
layout: single

title: python - opencv 

excerpt: "**¿Qué es opencv?**

OpenCV es una biblioteca libre desarrollada originalmente por Intel.
Escrita originalmente en C/C++, su mejor virtud es que es multiplataforma y se 
puede ejecutar en diferentes lenguajes de programación como Java, Objective C, Python y en C#.

La visión artificial o por computadora se está utilizando cada vez más 
para el análisis y tratamiento de imágenes mediante algoritmos de inteligencia 
artificial. Uno de los usos más importantes de OpenCV en la visión por 
computadora es la detección de rostros y objetos, sobre todo en ámbitos como 
la fotografía, el marketing o la seguridad."

date: 2022-05-27

classes: wide

header:

    teaser: ../assets/images/opencv/opencv.jpeg

    teaser_home_page: true
    
    icon: ../assets/images/logofairy.png

categories:

    - python3

tags:  

- opencv
---

![](../assets/images/opencv/Kursus-Python-OpenCV.jpg)

**¿Qué es opencv?**

OpenCV es una biblioteca libre desarrollada originalmente por Intel.
Escrita originalmente en C/C++, su mejor virtud es que es multiplataforma y se 
puede ejecutar en diferentes lenguajes de programación como Java, Objective C, Python y en C#.

La visión artificial o por computadora se está utilizando cada vez más 
para el análisis y tratamiento de imágenes mediante algoritmos de inteligencia 
artificial. Uno de los usos más importantes de OpenCV en la visión por 
computadora es la detección de rostros y objetos, sobre todo en ámbitos como 
la fotografía, el marketing o la seguridad.


## PASAR IMAGEN DE COLOR  A GRIS


```python

#!/bin/python3.9

import cv2

#"PATH DE LA IMAGEN", (0 = a gris). Por defecto 1,  muestra a color)
imagen = cv2.imread('wallpapers.png',0)

#nombre de la visualizacion
cv2.imshow('Prueba de imagen', imagen)

#Guardar IMAGEN
cv2.imwrite("test.png",imagen)

#tiempo de visualizacion(milisegundos)
cv2.waitKey(1000)

#cerrar las ventanas abiertas durante el proceso
cv2.destroyAllWindows()


```

## CAPTURAR IMAGENES CAMWEB


```python

#! /bin/python3.9


#importo libreria opencv
import cv2

#creo un objeto "VideoCapture" al cual le paso el indice de la camara.
captura = cv2.VideoCapture(0) #iniciar la captura

#Leer la imagen a cada momento
while (captura.isOpened()):

    #devuelve dos elemento True or False
    ret,imagen = captura.read()
    
    #si tenemos imagen
    if ret == True:
    
        #Que vamos a mostrar => imagen
        cv2.imshow("video",imagen)

        #opencv dice que usemos & 0xFF cuando usamos maquina de 64bits
        if cv2.waitKey(1) & 0xFF == ord('s'): #la tecla "s" detiene el programa

            break

#finalizamos la captura
captura.release()

#para cerrar cualquier ventana que puedo quedar abierta
cv2.destroyAllWindows()


```

## GRABAR VIDEO 

```python
#! /bin/python3.9


#importo libreria opencv
import cv2

#creo un objeto "VideoCapture" al cual le paso el indice de la camara.
captura = cv2.VideoCapture(0) #iniciar la captura

#VideoWriter => permite pasar 4 argumentos
#   *Nombre del Video
#   *codec del video => *'XVID' este es el que se encuentra en la documentacion
#   *fps
#   *tamaño del video

 
salida = cv2.VideoWriter("VideoSalida.mp4",cv2.VideoWriter_fourcc(*'XVID'), \
        60.0,(640,480))

#Leer la imagen a cada momento
while (captura.isOpened()):

    #devuelve dos elemento True or False
    ret,imagen = captura.read()
    
    #si tenemos imagen
    if ret == True:
    
        #Que vamos a mostrar => imagen
        cv2.imshow("video",imagen)
        #incluyo en la salida la imagen que la camara
        salida.write(imagen)
        #opencv dice que usemos & 0xFF cuando usamos maquina de 64bits
        if cv2.waitKey(1) & 0xFF == ord('s'): #la tecla "s" detiene el programa

            break

#finalizamos la captura
captura.release()
#Finalizar la grabacion
salida.release()
#para cerrar cualquier ventana que puedo quedar abierta
cv2.destroyAllWindows()

```

## REPRODUCIR VIDEO

```python
#! /bin/python3.9


#importo libreria opencv
import cv2

#creo un objeto "VideoCapture" al cual le paso el PATH del video.
captura = cv2.VideoCapture('VideoSalida.mp4') #reproducir video grabado

#Leer la imagen a cada momento
while (captura.isOpened()):

    #devuelve dos elemento True or False
    ret,imagen = captura.read()
    
    #si tenemos imagen
    if ret == True:
    
        #Que vamos a mostrar => imagen
        cv2.imshow("video",imagen)

        #opencv dice que usemos & 0xFF cuando usamos maquina de 64bits
        if cv2.waitKey(1) & 0xFF == ord('s'): #la tecla "s" detiene el programa

            break
    #termina de reproducir el video y termina el programa
    else:
        break
#finalizamos la captura
captura.release()

#para cerrar cualquier ventana que puedo quedar abierta
cv2.destroyAllWindows()

```


- [Repositorio](https://github.com/emablanco/opencv)
- [Documentacion](https://docs.opencv.org/3.0-beta/doc/py_tutorials/py_gui/py_video_display/py_video_display.html)

 
