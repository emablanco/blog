---
layout: single
title: Punteros C++
excerpt: "**Linsta Punteros**
Lorem ipsum dolor sit amet consecteyytur adipiscing elit, ligula non aenean vestibulum nunc praesent cras duis, eget sociosqu augue class magnis mattis. Semper magna nibh nisi sem posuere torquent senectus duis in, blandit nec et fusce magnis nisl cubilia mus massa lacinia, nam quisque velit eget euismod enim ultrices metus. Cras nam neque ultrices natoque eu integer egestas facilisi est, tempus sapien parturient ornare scelerisque molestie nec lacus condimentum iaculis, at torquent curae tortor nisl bibendum ad netus."

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


##  Ordenar elementos de una lista Lista 


Lorem ipsum dolor sit amet consecteyytur adipiscing elit, ligula non aenean vestibulum nunc praesent cras duis, eget sociosqu augue class magnis mattis. Semper magna nibh nisi sem posuere torquent senectus duis in, blandit nec et fusce magnis nisl cubilia mus massa lacinia, nam quisque velit eget euismod enim ultrices metus. Cras nam neque ultrices natoque eu integer egestas facilisi est, tempus sapien parturient ornare scelerisque molestie nec lacus condimentum iaculis, at torquent curae tortor nisl bibendum ad netus."

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
## Eliminar Elemento de Una Lista

Lorem ipsum dolor sit amet consecteyytur adipiscing elit, ligula non aenean vestibulum nunc praesent cras duis, eget sociosqu augue class magnis mattis. Semper magna nibh nisi sem posuere torquent senectus duis in, blandit nec et fusce magnis nisl cubilia mus massa lacinia, nam quisque velit eget euismod enim ultrices metus. Cras nam neque ultrices natoque eu integer egestas facilisi est, tempus sapien parturient ornare scelerisque molestie nec lacus condimentum iaculis, at torquent curae tortor nisl bibendum ad netus."

```c++

#include <iostream>

using namespace std;

struct  datos{

    int num;

    datos * siguiente;
};

datos * cargarDatos(datos * inicio , int elemento){

    datos * nuevo = new datos;

    nuevo -> num = elemento;

    nuevo -> siguiente = inicio;

    return nuevo;

}

datos * eliminarElemento(datos * inicio , int numero){
    
    if (inicio != nullptr){
        
        datos * aux = inicio;
                
        if (inicio -> num == numero){

            inicio = inicio -> siguiente;

            delete aux;
        }

        else{

            while (aux -> siguiente != nullptr and aux -> siguiente -> num != numero){

                aux = aux -> siguiente;

            }

            if (aux -> siguiente != nullptr and aux -> siguiente -> num == numero){

                datos * eliminar = aux -> siguiente;

                aux -> siguiente = aux -> siguiente -> siguiente;

                delete eliminar;
            }
        }
    }

    return inicio;
}

void imprimirLista(datos * );

int main(){

    datos * ema = nullptr;

    for (int i = 0; i < 10; i++){
        
        int numero = rand()% (100 - 1) +1;
        
        ema = cargarDatos(ema, numero);
    }

    imprimirLista(ema);

    int n;
    
    cout <<"Ingresar Valor a Eliminar: ";cin>>n;
    
    ema = eliminarElemento(ema , n);

    imprimirLista(ema);

    return 0;
}

void imprimirLista( datos * lista){

    for (datos * i = lista; i -> siguiente != nullptr; i = i -> siguiente){
        
        cout << i -> num <<endl;
    }
    
}

```
