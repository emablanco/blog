---
layout: single
title: Linux - Bomba fork 
excerpt: "**¿Qué es una bomba fork?**
Una bomba fork es un tipo de ataque del tipo denegación de servicio sobre un 
computador implementando una operación fork o alguna otra funcionalidad 
equivalente mediante la cual un proceso es capaz de autorreplicarse.
La bomba fork es considerado un wabbit, ya que no se autorreplica de la misma 
forma que los gusanos o los virus."

date: 2022-05-29
classes: wide
header:
  teaser: ../assets/images/bomba/bomba.jpg
  teaser_home_page: true
  icon: ../assets/images/logofairy.png
categories:
  - Denegacion de servicio
tags:  
  - c++
  - bash
---

![](../assets/images/bomba/fork.jpg)


Una bomba fork funciona creando una gran cantidad de procesos muy rápidamente 
con el objetivo de saturar el espacio disponible en la lista de procesos 
mantenida por el sistema operativo del computador. 
Si la tabla de procesos se llega a saturar, entonces no se pueden iniciar 
nuevos programas hasta que no se cierre alguno. En el caso que esto suceda, es
muy poco probable que se pueda iniciar un programa útil ya que los procesos de
la bomba estarán esperando para poder crear nuevos procesos a la primera 
oportunidad que se les conceda.

Las bombas fork no sólo ocupan espacio dentro de la lista de procesos, también 
consumen tiempo de proceso y memoria de la máquina donde se ejecutan. 
Como resultado de esto, los ordenadores se vuelven lentos e incluso se pueden 
volver inutilizables dada la falta de memoria y la imposibilidad de aprovechar
el procesador.

## Bomba Fork en C++

Para utilizar la funcion fork en C o C++ se debe incluir la libreria <unistd.h>


![](../assets/images/bomba/c++.png)

```c++

#include <unistd.h>

int main(){

    while(true) fork();

}

```

## Bomba Fork en BASH


Para crear una bomba fork en bash solo se debe crear una función vacía, 
que se llame así misma y su salida sea redirigida a la misma función, 
la cual se ejecutara en segundo plano. O sea, cada vez que se llame la función 
se generara un nuevo proceso que correra en segundo plano.



```bash
bomba_fork() { bomba_fork|bomba_fork & }; bomba_fork

```

![](../assets/images/bomba/bash.png)


## Forma mas simplificada


```bash

:() { : | :& } ; :

```

## Como evitar este ataque en Linux


En mi test de la bomba fork para evitar que me cuelgue el sistema, solo le permití 
a el usuario «servidor» que pueda ejecutar 15 procesos como máximo. 
Si se observa en la imagen donde ejecuto la bomba fork en bash, se interrumpió 
la ejecucion luego de que se crearan 15 procesos.

Para realizar esta configuración solo se debe editar el archivo 
«/etc/security/limits.conf». Donde se debe definir la cantidad de procesos que 
se le permitirá ejecutar a cada usuario del sistema.

![](../assets/images/bomba/security.png)
