---
layout: single
title: Punteros C++
excerpt: "**Listas Enlazadas ->**

Las listas enlazadas mediante punteros permiten un manejo de memoria a bajo nivel.
Al trabajar con punteros, es fundamental conocer las diferencias entre memoria stack
y memoria heap.
Las listas enlazadas son las más sencillas para comenzar con el tema. De ellas 
se derivan luego otras estructuras con punteros, como las listas doblemente enlazadas, 
listas circulares, los grafos, y los árboles.

Mediante este ejemplo se puede entender como funciona la memoria a bajo nivel,
qué son los nodos, cómo se crean y cómo se accede a los datos de un nodo, 
cómo almacenar datos en una lista, cómo definir una lista , cómo recorrer una lista,
cómo buscar un elemento en una lista."

date: 2022-06-12
classes: wide
header:
  teaser: /assets/images/EjerciciosC++/c++.jpg
  teaser_home_page: true
  icon: /assets/images/logofairy.png
categories:
  - Punteros C++
tags:  
  - c++
---

![](/assets/images/EjerciciosC++/ListasSimplementeEnlazadas.jpg)



## Memoria Stack

Este sector se utiliza para crear variables simples de **Tipos primitivos:** (int, short, long, double, float, boolean, byte, char.),
 que no son punteros. Normalmente tienen un espacio limitado y fijo 
que se asigna al iniciar la ejecucion del programa. Este espacio no puede crecer.

La alocacion se realiza de manera mas rapida que en la Heap, los datos almacenados pueden usarse sin punteros o con punteros 
y las variables se desalojan automaticamente.


## Memoria Heap

La memoria dinámica que se almacena en el heap es aquella que se utiliza 
para almacenar datos que se crean en medio de la ejecución de un programa, este espacio 
puede llegar a crecer a medida que se requiera mas espacio. 
En la Heap la alocacion es mas lenta que en la stack, a los datos solo se pueden acceder mediante
punteros y se manejan manualmente (new / delete). El Almacenamiento es no continuo lo que puede llegar a producir fragmentacion.

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

## Ejercicio Lista Enlazadas

Desarrollar un programa que realice las siguientes operaciones en una listas de numeros enteros.

A) Cargar una lista insertando numeros ordenados en forma ascendente. Finalizar cuando se ingrese un 0 (cero), que no se debe tener en cuenta.

B) Eliminar las ocurrencias de los numeros que son multimplos de 3. imprimir la lista antes y despues de la eliminacion.


```c++

#include <iostream>

using namespace std;

struct datos{
    
    int num;

    datos * siguiente;
    
};

datos * cargarDatos(datos * lista , int numero);

datos * eliminarMultiplos(datos * lista , int numero);

void imprimir(datos * lista);

int main(){
    
    datos * lista = nullptr;

    int elemento;
   
    cout <<"Ingresar Numero (0 para cortar): ";cin >> elemento;

    while(elemento != 0){

        lista = cargarDatos(lista , elemento);

        cout <<"Ingresar Numero (0 para cortar): ";cin >> elemento;

    }
    
    cout <<"Lista Ordenada de forma ascendente "<<endl;
   
    imprimir(lista);
   
    cout <<"Multiplos a eliminar: ";cin>> elemento;

    lista = eliminarMultiplos(lista , elemento);
   
    imprimir(lista);
   
    return 0;
}

datos * cargarDatos(datos * lista , int numero){

    datos * nuevo = new datos;

    nuevo -> num = numero;

    nuevo -> siguiente = nullptr;
    
    if (lista == nullptr or numero < lista -> num){

        nuevo -> siguiente = lista;
        
        lista = nuevo;

    }
    else{
        
        datos * aux = lista;

        while(aux -> siguiente != nullptr and aux -> siguiente -> num < numero){
                
            aux = aux -> siguiente;

        }

        if (aux -> siguiente != nullptr){

            nuevo -> siguiente = aux -> siguiente;

        }
            
        aux -> siguiente = nuevo;

    }

    return lista;

}

datos * eliminarMultiplos(datos * lista , int numero){

    datos * eliminar;

    datos * aux = lista;

    while(aux != nullptr){
        
        if (lista -> num % numero == 0){
            
            eliminar = lista;

            lista = lista -> siguiente;
            
            aux = aux -> siguiente;

            delete eliminar;

        }

        else{
            
            if (aux -> siguiente != nullptr and aux -> siguiente -> num % numero == 0){
    
                eliminar = aux -> siguiente;

                aux -> siguiente = aux -> siguiente -> siguiente;

                delete eliminar;
        
            }

            else{
                
                aux = aux -> siguiente;
            }
        }
    }

    return lista;
}

void imprimir(datos * lista){
    
    int contador = 0;

    for (datos * i = lista; i != nullptr; i = i -> siguiente){

        cout <<"["<<contador<<"]"<<i -> num <<endl;
        
        contador++;
    }
}

```
## Terminar POST
