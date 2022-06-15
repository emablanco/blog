---
layout: single
title: Punteros C++
excerpt: "**Linsta Punteros**

Las listas enlazadas mediante punteros permiten un manejo de memoria a bajo nivel.
Al trabajar con punteros, es fundamental conocer las diferencias entre memoria stack
y memoria heap.
Las listas enlazadas son las más sencillas para comenzar con el tema. De ellas 
se derivan luego otras estructuras con punteros, como las listas doblemente enlazadas, 
listas circulares, los grafos, y los árboles.

Mediante este ejemplo se puede entender como funciona la memoria a bajo nivel,
qué son los nodos, cómo se crean y cómo se accede a los datos de un nodo, 
cómo almacenar datos en una lista, cómo definir una lista , cómo recorrer una lista,
cómo buscar un elemento en una lista.

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

![](/assets/images/EjerciciosC++/ListasSimplementeEnlazadas.jpg)


##  Practica de lista Enlazadas 

Las listas enlazadas mediante punteros permiten un manejo de memoria a bajo nivel.
Al trabajar con punteros, es fundamental conocer las diferencias entre memoria stack
y memoria heap.
Las listas enlazadas son las más sencillas para comenzar con el tema. De ellas 
se derivan luego otras estructuras con punteros, como las listas doblemente enlazadas, 
listas circulares, los grafos, y los árboles.

Mediante este ejemplo se puede entender como funciona la memoria a bajo nivel,
qué son los nodos, cómo se crean y cómo se accede a los datos de un nodo, 
cómo almacenar datos en una lista, cómo definir una lista , cómo recorrer una lista,
cómo buscar un elemento en una lista.

```c++

#include <iostream>

using namespace std;

struct datos {

    int num;

    datos * siguiente;

};

datos * cargarDatos(datos * inicio, int numero);

datos * cargarDatosAlFinal(datos * inicio, int numero);

datos * cargarDatosOrdenados(datos * inicio , int numero);

datos * ordenarDatosMayorMenor(datos * inicio);

datos * eliminarElemento(datos * inicio , int elemento);

void imprimirLista(datos * inicio);

int main(){

    datos * test1 = nullptr;
    datos * test2 = nullptr;
    datos * lista = nullptr;

    int elementos;

    cout <<"Cuntos Numeros vas a ingresar: ";cin>>elementos;
    
    for (int i = 0 ; i < elementos ; i++){
        
        int n;
        
        cout << "Ingresar Numero: ";cin>>n;

        test1 = cargarDatos(test1 , n);
    }
    
    cout <<"Datos cargados en orden inverso "<<endl;

    imprimirLista(test1);
 
    for (int i = 0 ; i < elementos ; i++){
        
        int n;
        
        cout << "Ingresar Numero: ";cin>>n;

        test2 = cargarDatosAlFinal(test2 , n);
    }
    
    cout <<"Datos cargados en orden de llegada "<<endl;

    imprimirLista(test2);
 
    cout <<"Ingresar numeros aleatorios: "<<endl; 
    
    for (int i = 0 ; i < elementos ; i++){
        
        int n;
        
        cout << "Ingresar Numero: ";cin>>n;

        lista = cargarDatosOrdenados(lista , n);
    }
    
    cout <<"Datos Ordenados de Menor a Mayor"<<endl;

    imprimirLista(lista);

    cout <<"Ordenados de Mayor a Menor"<<endl;

    lista = ordenarDatosMayorMenor(lista);
    
    imprimirLista(lista);

    int eliminar;

    cout <<"Elemento a eliminarElemento: ";cin>>eliminar;

    lista = eliminarElemento(lista, eliminar);

    imprimirLista(lista);

    return 0;
}

datos * cargarDatos(datos * inicio, int numero){

    datos * nuevo = new datos;

    nuevo -> num = numero;

    nuevo -> siguiente = inicio;

    return nuevo;
}
datos * cargarDatosAlFinal(datos * inicio, int numero){

    datos * nuevo = new datos;

    nuevo -> num = numero;

    nuevo -> siguiente = nullptr;

    if (inicio == nullptr){

        inicio = nuevo;
    }
    else{

        datos * aux = inicio;
        
        while(aux -> siguiente != nullptr){

            aux = aux -> siguiente;
        }

        aux -> siguiente = nuevo;

    }

    return inicio;
}

datos * cargarDatosOrdenados(datos * inicio , int numero){

    datos * nuevo = new datos;

    nuevo -> num = numero;

    nuevo -> siguiente = nullptr;
    
    if (inicio == nullptr or numero < inicio -> num){

        nuevo -> siguiente = inicio;

        inicio = nuevo;
    }

    else{
        
        datos * aux = inicio;

        while(aux -> siguiente != nullptr and aux -> siguiente -> num < numero){
            
            aux = aux -> siguiente;
            
        }
        
        if (aux -> siguiente != nullptr){
            
            nuevo -> siguiente = aux -> siguiente;

        }
        
        aux -> siguiente = nuevo;

    }

    return inicio;


}

datos * ordenarDatosMayorMenor(datos * inicio){

    for(datos * i = inicio; i != nullptr; i = i -> siguiente){
        
        for(datos * e = i -> siguiente ; e != nullptr; e = e -> siguiente){
            
            if (i -> num < e -> num){
                
                int swap = i -> num;

                i -> num = e -> num;

                e -> num = swap;
            }
        }
    }

    return inicio;

}

datos * eliminarElemento(datos * inicio , int elemento){

    if (elemento == inicio -> num){        
        
        datos * eliminar = inicio;

        inicio = inicio -> siguiente;

    }

    else{

        datos * aux = inicio;

        while(aux -> siguiente != nullptr and elemento != aux -> siguiente -> num){

            aux = aux -> siguiente;
        }

        if (aux -> siguiente != nullptr){
        
            datos * eliminar = aux -> siguiente;

            aux -> siguiente = aux -> siguiente -> siguiente;

            delete eliminar;

        }
    }
    return inicio;
}

void imprimirLista(datos * inicio){

    int contador = 0;

    for (datos * i = inicio ; i != nullptr; i = i -> siguiente){
        
        cout <<"["<<contador<<"] "<<i -> num <<endl;

        contador++;
    }
}

```
## Terminar post
