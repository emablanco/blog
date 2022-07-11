---
layout: single
title: Linux - Sistema de Vigilancia 
excerpt: "**¿Qué es un Sistema de Videovigilancia?**

Los Sistemas de Vigilancia permiten comprobar desde otra ubicación, el funcionamiento o el estado de un lugar"
date: 2022-07-11
classes: wide
header:
  teaser: /assets/images/bomba/bomba.jpg
  teaser_home_page: true
  icon: /assets/images/logofairy.png
categories:
  - Seguridad
  - Sistemas de Videovigilancia
  - Linux
  - MotionEye
tags:  
  - Seguridad
  - Videovigilancia
---

```yaml
version: "3"
services:
  motioneye:
    image: ccrisan/motioneye:master-amd64 #funciona para grabar
    image: ccrisan/motioneye:dev-amd64 #me da problemas para grabar
    container_name: motioneye
    volumes:
      - /etc/localtime:/etc/localtime:ro #Timezone Config / Do Not Change
      - ./configuracion:/etc/motioneye #Config Storage
      - ./archivos:/var/lib/motioneye  #File Storage
    ports:
      - 8765:8765
    restart: always


```
