---
layout: single
title: Fundamentos de Programacion C++
excerpt: "**Ejercicios**

Considere el struct atleta { string nombre; int numero, país; hh,mm,ss; } entre
cuyos miembros se define el tiempo empleado en una prueba de ciclismo en la que competían 10
países. En la prueba participaron 2 ciclistas por cada país.
a) Escriba un programa C++ que lea los datos (nombre, número, código país (1..10), hh, mm, ss) de
los 20 participantes de la prueba, y los organice en un vector. En el caso de que un competidor no
haya finalizado la prueba, se ingresará el tiempo 0:0:0.
b) Escriba una función extraer_mejor que reciba el vector con los datos de todos los ciclistas,
encuentre el de mejor tiempo entre ellos, lo elimine del vector, y lo retorne a la función main.
c) Determine el país ganador, que será aquel que haya obtenido el mayor puntaje sumando los
puntajes que lograron sus 2 participantes. La prueba otorga 20 puntos al ganador, 15 puntos al
segundo, 11 puntos al tercero, 8 puntos al cuarto y 5 puntos al quinto. Utilice la función extraer_mejor
5 veces para obtener los 5 primeros. Para el resto, se otorga 1 punto si el competidor finalizó la
prueba y 0 si debió abandonar. En caso de empate entre dos o más países, solo debe informarlo con
un mensaje alusivo."

date: 2022-05-29
classes: wide
header:
  teaser: /assets/images/EjerciciosC++/c++.png
  teaser_home_page: true
  icon: /assets/images/logofairy.png
categories:
  - Fundamentos de programacion
tags:  
  - c++
---

![](/assets/images/EjerciciosC++/Ejercicio1.png )

## Ejercicio 1  

Considere el struct atleta { string nombre; int numero, país; hh,mm,ss; } entre
cuyos miembros se define el tiempo empleado en una prueba de ciclismo en la que competían 10
países. En la prueba participaron 2 ciclistas por cada país.
a) Escriba un programa C++ que lea los datos (nombre, número, código país (1..10), hh, mm, ss) de
los 20 participantes de la prueba, y los organice en un vector. En el caso de que un competidor no
haya finalizado la prueba, se ingresará el tiempo 0:0:0.
b) Escriba una función extraer_mejor que reciba el vector con los datos de todos los ciclistas,
encuentre el de mejor tiempo entre ellos, lo elimine del vector, y lo retorne a la función main.
c) Determine el país ganador, que será aquel que haya obtenido el mayor puntaje sumando los
puntajes que lograron sus 2 participantes. La prueba otorga 20 puntos al ganador, 15 puntos al
segundo, 11 puntos al tercero, 8 puntos al cuarto y 5 puntos al quinto. Utilice la función extraer_mejor
5 veces para obtener los 5 primeros. Para el resto, se otorga 1 punto si el competidor finalizó la
prueba y 0 si debió abandonar. En caso de empate entre dos o más países, solo debe informarlo con
un mensaje alusivo."






```c++

#include <iostream>

#include <vector>

using namespace std;

struct atletas {string nombre; int pais , numero , hh, mm ,ss;};

void cargarDatos(vector <atletas> &);

atletas extraerMejor(vector <atletas> &);

void ganadorPais(vector <atletas> &, atletas ganador);

int main(){

    vector <atletas> carrera;
    cargarDatos(carrera);
    
    atletas ganador;

    ganador = extraerMejor(carrera);
    
    ganadorPais(carrera, ganador);

    return 0;

}

void cargarDatos(vector <atletas> &carrera){
    
    atletas datos;

    carrera.resize(20);

    for (size_t i = 0; i < carrera.size(); i++){
        cout<<"Participante N°: "<<i+1<<endl;
        cout <<"Nombre: ";
        getline(cin, datos.nombre);
        cout<<"Numero del participante: ";cin>>datos.numero;
        cout <<"Pais N°: ";cin>>datos.pais;
        cout <<"HH: ";cin>>datos.hh;
        cout <<"MM: ";cin>>datos.mm;
        cout <<"SS: ";cin>>datos.ss;
        carrera[i] = datos;
        cin.ignore();
        cout<<endl;
    }
    
}
atletas extraerMejor(vector <atletas> &carrera){
    
    atletas eliminar;
    
    int tiempo = 0, index = 0;
    tiempo += carrera[0].hh * 60 * 60;
    tiempo += carrera[0].mm * 60;
    tiempo += carrera[0].ss;

    for (size_t i = 0; i < carrera.size(); i++ ){
        
        int tiempo2 = 0;
        tiempo2 += carrera[i].hh * 60 * 60;
        tiempo2 += carrera[i].mm * 60;
        tiempo2 += carrera[i].ss;
        
        if (tiempo2 < tiempo){
            tiempo = tiempo2;
            index = i;
        }
        
    }

    eliminar = carrera[index];
    
    swap (carrera[index], carrera[carrera.size()-1]);
    
    carrera.pop_back();

    return eliminar;

}

void ganadorPais(vector <atletas> &c, atletas ganador){

    vector <atletas> carrera;
    
    carrera = c;

    atletas datos;

    vector <atletas> ganadores(4);

    int puntos_win = 20, pais = ganador.pais, puntos_perdedor = 15, pais2 = ganadores[0].pais;

    for (size_t i = 0; i < ganadores.size(); i++){
        
        datos  = extraerMejor(carrera);
        
        ganadores[i] = datos;

    }
    
    if (ganador.pais == ganadores[0].pais){
        
        cout <<"El pais ganador es: "<<ganador.pais<<endl;
    }
     if (ganador.pais == ganadores[1].pais){

        cout <<"El pais ganador es: "<<ganador.pais<<endl;
     }
    if (ganador.pais == ganadores[2].pais){

        cout <<"El pais ganador es: "<<ganador.pais<<endl;

    }
    else{

        for (size_t i = 1 ; i < ganadores.size(); i++){
            
            if (ganadores[0].pais == ganadores[i].pais){
                
                switch (i) {
                    
                    case 1:
                        puntos_perdedor += 11;
                        break;

                    case 2:
                        puntos_perdedor += 8;
                        break;

                    case 3:
                        puntos_perdedor += 5;
                        break;

                }

            }
        }

        if (puntos_perdedor > puntos_win){

            cout <<"El pais ganador es: "<<ganadores[0].pais;
            cout <<"Puntos: "<<puntos_perdedor;
        }
        
        else{

            if(puntos_win == puntos_perdedor){
                
                int t;

                for(size_t i = 0; i < carrera.size(); i++){
                    
                        if (ganador.pais == carrera[i].pais){
                
                            t = carrera[i].hh + carrera[i].mm + carrera[i].ss;

                            }
                } 

                if (t > 0){

                    cout <<"El ganador de la carrera es "<<ganador.pais;

                    }

            }

            if (puntos_perdedor < puntos_win){

                cout <<"Ganador "<<ganador.pais;
            }

            else{

                cout<<"Empate entre pais "<<ganador.pais<<" y "<<ganadores[0].pais;
            }
        
            
        }
        
    }
}

```


## Ejercicio 2 

a) Escriba una función frecuencias(…) que reciba dos vectores de enteros v1 y
v2 de igual tamaño. La función debe calcular la frecuencia (cuántas veces se repite) de cada
elemento de v1 en el mismo vector v1. Las frecuencias de los elementos de v1 conformarán el
arreglo v2. Es decir, el i-ésimo elemento de v2 deberá decir cuántas veces se repite el i-ésimo
elemento de v1.

b) Escriba una función mayor_frecuencia(…) que utilizando el vector modificado por frecuencias()
retorne el valor que más se repite en el mismo. Por ejemplo, si el primer arreglo es {1,5,1,9,8,9,9,12},
la función frecuencias retornará {2,1,2,3,1,3,3,1} (el 1 está 2 veces, el 9 está 3 veces, los demás solo
una vez), la función mayor_frecuencia deberá retornar 9.

c) Escriba un programa cliente que utilice ambas funciones.

```c++

#include <iostream>

#include <vector>

using namespace std;

void cargarDatos(vector <int> &, vector <int> &);


void frecuencia(vector <int> &, vector <int> &);


void mayorFrecuencia(vector <int> &, vector <int> &);


int main(){
    
    vector <int> v1;
    vector <int> v2;

    cargarDatos(v1,v2);
    frecuencia(v1,v2);

    mayorFrecuencia(v1,v2);
    
    return 0;
}

void cargarDatos(vector <int> &v1, vector <int> &v2){

    int n;
    
    cout <<"Cantidad de elementos para el vector: ";cin>>n;
    
    v1.resize(n);
    v2.resize(n); 
    
    for (size_t i = 0 ; i < v1.size(); i++){
    
        v1[i] = rand() % (20 - 1) + 1;
    }
    
    for (size_t i = 0; i < v2.size(); i++){
        
        v2[i] = 0;
    }

    
}


void frecuencia(vector <int> &v1, vector <int> &v2){

    for (size_t i = 0; i < v1.size(); i++){

        for (size_t e = 0; e < v1.size(); e++){

            if (v1[e] == v1[i]){
                
                v2[i] += 1;

            }
        }
    }

    for (size_t i = 0 ; i < v1.size(); i++){

        cout<<v1[i] << " = "<<v2[i]<<endl;
    }
}


void mayorFrecuencia(vector <int> &v1, vector <int> &v2){
    
    
    for (size_t i = 0; i < v2.size(); i++){
        
        for (size_t e = 0; e < v2.size(); ++e){

            if (v2[i] > v2[e]){
                
                swap(v1[i],v1[e]);
                swap(v2[i],v2[e]);
            }
        } 
    }

    cout<<endl;
    cout <<v1[0] <<" = "<<v2[0];
    
    for (size_t i = 0; i < v2.size(); ++i){
        
        if (v2[i] == v2[0] and v1[i] != v1[0]){
            
            cout<<endl;

            cout << v1[i]<<" = "<<v2[i];
        }
    }
}

```
