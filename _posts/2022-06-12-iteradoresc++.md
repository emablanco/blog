---
layout: single
title: Iteradores C++
excerpt: "**Recorrer Vectores, mapas o conjuntos**

Recorrer vectores, mapas o conjuntos, sin preocuparnos por los índices o los elementos que contengan, es mediante el uso de iteradores.

"

date: 2022-06-19
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

![](/assets/images/EjerciciosC++/iterador.png)

Recorrer vectores y  mapas sin conocer la cantidad de elementos que contengan,

```c++

#include <iostream>
#include <string>
#include <map>
#include <vector>

using namespace std;

struct Persona{
    string nombre;
    int legajo;
};


map <string, vector <Persona>> cargarDatos(map <string, vector<Persona>> );

void imprimir(map <string, vector <Persona>> );

vector <Persona> obtenerPersonal(vector <Persona> );

vector <Persona> ordenarLegajos(vector <Persona> , Persona );

vector <Persona> eliminar(vector <Persona> , int );

int contarLetras(map <string, vector <Persona>> ,char);

int main(){
    
    map <string, vector <Persona>> empresa;

    empresa = cargarDatos(empresa);
    
    imprimir(empresa);
   
//--------------------------------------------------------------
    char letra;

    cout <<"Contar cantidad de nombres que comiencen por la Letra: ";cin>>letra;

    int letras;

    letras = contarLetras(empresa, letra);

    cout <<"Cantidad de Nombres que comienzan por la Letra "<<letra<<": "<<letras;

    cout<<endl;

//--------------------------------------------------------------

    cout <<"\nOrdenar Legajos: \n"<<endl;
    
    vector <Persona> area;
    
    string a;
    
    cout <<"Area a Filtrar: ";cin>>a;
    
    area = obtenerPersonal(empresa[a]);

    cout<<endl;
    
    for (auto i = begin(area); i != end(area); i++){

        cout <<"Nombre: "<<(*i).nombre<< " Legajo: "<<(*i).legajo<<endl;
    }
    
    cout<<endl;

//--------------------------------------------------------------
    int num;

    cout <<"Eliminar Legajos que finalicen con el digito: "; cin>> num;

    area = eliminar(area , num);
    
    cout<<endl;
   
    for (auto i = begin(area); i != end(area); i++){

        cout <<"Nombre: "<<(*i).nombre<< " Legajo: "<<(*i).legajo<<endl;
    }

    return 0;
}

int contarLetras(map <string, vector <Persona>> empresa,char letra){
    
    int contador = 0;

    for (auto vec: empresa){

        for( Persona persona  : vec.second){

            if (persona.nombre[0] == letra ){

                contador++;
            }
        }
    }
    return contador;
}

vector <Persona> eliminar(vector <Persona> empresa, int num){

    auto i = begin(empresa);

    while(i != end(empresa)){

        if(i -> legajo % 10 == num){

            i = empresa.erase(i);
        }

        else{
        
            i++;

        }
    }

    return empresa;
}
vector <Persona> ordenarLegajos(vector <Persona> nuevo , Persona p){

    auto i = begin(nuevo);

    while( i != end(nuevo) and i -> legajo <= p.legajo){

        i++;
    }

    nuevo.insert(i, p);

    return nuevo;
}

vector <Persona> obtenerPersonal(vector <Persona> empresa){

    vector <Persona> nuevo;

    for(auto i = begin(empresa); i != end(empresa); i++){
        
        nuevo = ordenarLegajos(nuevo, *i);

    }

    return nuevo;
}

void imprimir(map <string, vector <Persona>> empresa){

    for(auto i = begin(empresa); i != end(empresa); i++){

        cout << "***Area: " <<i -> first << "****\n" << endl;
        
        for(auto e = begin( i -> second); e != end(i -> second); e++){

            cout << "Nombre: "<<e -> nombre<<" Legajo: "<< e -> legajo<<endl;

        }

        cout <<"\n"<<endl;
    }
}

map <string, vector <Persona>> cargarDatos(map <string, vector<Persona>> contenedor){
    
    Persona persona;

    persona.nombre="Emanuel Blanco ";
    persona.legajo=19;
    contenedor["Administracion"].push_back(persona);

    persona.nombre="Claudia River";
    persona.legajo=154;
    contenedor["Administracion"].push_back(persona);

    persona.nombre="Rey Lag";
    persona.legajo=129;
    contenedor["Administracion"].push_back(persona);

    persona.nombre="Fernanda Stallaman";
    persona.legajo=124;
    contenedor["Administracion"].push_back(persona);

    persona.nombre="El noba";
    persona.legajo=69;
    contenedor["Administracion"].push_back(persona);

    persona.nombre="Alma Marcela goso";
    persona.legajo=189;
    contenedor["Administracion"].push_back(persona);

    persona.nombre="Joaquin Ramos";
    persona.legajo=426;
    contenedor["Produccion"].push_back(persona);

    persona.nombre="Ricardo Gimenez";
    persona.legajo=165;
    contenedor["Administracion"].push_back(persona);

    persona.nombre="Jessica Williams";
    persona.legajo=329;
    contenedor["Capacitaciones"].push_back(persona);

    persona.nombre="Jonathan Rojas";
    persona.legajo=253;
    contenedor["Produccion"].push_back(persona);

    persona.nombre="Julia Dominguez";
    persona.legajo=397;
    contenedor["Capacitaciones"].push_back(persona);

    persona.nombre="Rodolfo Quinteros";
    persona.legajo=562;
    contenedor["Produccion"].push_back(persona);

    return contenedor; 

}


```
