---
layout: single
title: Iteradores C++
excerpt: "**Recorrer Vectores, mapas o conjuntos**

Recorrer vectores, mapas o conjuntos, sin preocuparnos por los índices o los elementos que contengan, es mediante el uso de iteradores.

"

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

![](/assets/images/EjerciciosC++/iterador.png)

Recorrer vectores, mapas o conjuntos, sin preocuparnos por los índices o los elementos que contengan, es mediante el uso de iteradores.

```c++


#include <iostream>
#include <vector>
#include <map>

using namespace std;

struct datos{
    
    int edad;

    string nombre;

};

map <string, datos> cargarDatos(map <string, datos> empresa);

vector <datos> cargarDatos2(vector <datos> empresa2);

int main(){

    map <string , datos> empresa;
    vector <datos> empresa2;

    empresa = cargarDatos(empresa);
    empresa2 = cargarDatos2(empresa2);
    
    cout <<"map\n"<<endl;
    for(auto i = begin(empresa); i != end(empresa); i++){
        
        cout <<"**********"<< i -> first <<"**********"<<endl;
        cout <<"Nombre: "<< i -> second.nombre<<endl;
        cout<<"Edad: "<<i -> second.edad << endl;
        cout<<endl;
    }
   
    cout <<"Vector\n"<<endl; 
    
    for(auto i = begin(empresa2); i != end(empresa2); i++){
        
        cout << "Nonbre: " << (*i).nombre<<endl;
        cout <<"Edad: " << (*i).edad << endl;

    }

    return 0;
}

vector <datos> cargarDatos2(vector <datos> empresa2){
        
    datos aux;

    aux.edad = 21;
    aux.nombre = "Emanuel Rech";
    empresa2.push_back(aux);
    
    aux.edad = 21;
    aux.nombre = "Alendro Varistock";
    empresa2.push_back(aux);
    
    aux.edad = 41;
    aux.nombre = "Matias Retamal";
    empresa2.push_back(aux);
    
    aux.edad = 25;
    aux.nombre = "Ariel otamendy";
    empresa2.push_back(aux);
    
    aux.edad = 24;
    aux.nombre = "Alexis Nuñes";
    empresa2.push_back(aux);
    
    aux.edad = 28;
    aux.nombre = "Emanuel ravistivich";
    empresa2.push_back(aux);

    return empresa2;

}

map <string, datos> cargarDatos(map <string, datos> empresa){

    datos aux;

    aux.edad = 21;
    aux.nombre = "Emanuel Rech";
    empresa["expedicion"] = aux;
    aux.edad = 21;
    aux.nombre = "Alendro Varistock";
    empresa["Produccion"] = aux;
    aux.edad = 41;
    aux.nombre = "Matias Retamal";
    empresa["supervisor"] = aux;
    aux.edad = 25;
    aux.nombre = "Ariel otamendy";
    empresa["Ingeniero"] = aux;
    aux.edad = 24;
    aux.nombre = "Alexis Nuñes";
    empresa["Mantenimiento"] = aux;
    aux.edad = 28;
    aux.nombre = "Emanuel ravistivich";
    empresa["Encargado Produccion"] = aux;

    return empresa;
}


```
