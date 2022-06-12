---
layout: single
title: Punteros C++
excerpt: "**Linsta Punteros**
Lorem ipsum dolor sit amet consectetur adipiscing elit, ligula non aenean vestibulum nunc praesent cras duis, eget sociosqu augue class magnis mattis. Semper magna nibh nisi sem posuere torquent senectus duis in, blandit nec et fusce magnis nisl cubilia mus massa lacinia, nam quisque velit eget euismod enim ultrices metus. Cras nam neque ultrices natoque eu integer egestas facilisi est, tempus sapien parturient ornare scelerisque molestie nec lacus condimentum iaculis, at torquent curae tortor nisl bibendum ad netus."

date: 2022-06-12
classes: wide
header:
  teaser: /assets/images/EjerciciosC++/c++.png
  teaser_home_page: true
  icon: /assets/images/logofairy.png
categories:
  - Punteros C++
tags:  
  - c++
---

![](/assets/images/EjerciciosC++/c++.png )

## Lista 


```c++
#include <iostream>

using namespace std;

struct  datos{

    int num;

    datos * siguiente;
};

datos * insertarOrden(datos * inicio , int elemento){

    datos * nuevo = new datos;

    nuevo -> num = elemento;

    nuevo -> siguiente = nullptr;

    if (inicio == nullptr || elemento < inicio -> num){

        nuevo -> siguiente = inicio;

        inicio = nuevo;
    }
    
    else{

        datos * aux = inicio;

        while(aux -> siguiente != nullptr && aux -> siguiente -> num < elemento){

            aux = aux -> siguiente;
            
        }

        if (aux -> siguiente != nullptr){

            nuevo -> siguiente = aux -> siguiente;
        }

        aux -> siguiente = nuevo;
    }

    return inicio;

}

int main(){

    datos * ema = nullptr;

    for (int i = 0; i < 10; i++){
        
        int numero = rand()% (100 - 1) +1;
        
        ema = insertarOrden(ema, numero);
    }

    for (datos * i = ema; i -> siguiente != nullptr; i = i -> siguiente){
        
        cout << i -> num <<endl;
    }
        
    return 0;
}
```
