---
layout: single
title:  ESP32 - SERVIDOR WEB
excerpt: "En ocasiones es muy práctico publicar el resultado de unos sensores en la web para poder consultarlos en remoto. Para ello montare un pequeño circuito con un Sensor de temperatura DHT11 y que se puedan ver su valores desde el navegador web."

date: 2023-03-23
classes: wide
header:
    teaser: ../assets/images/esp32-web/logo.jpg
    teaser_home_page: true
    icon: ../assets/images/logofairy.png
categories:
    - Debian
tags:  
 - Programaciom
 - arduino
 - C++
---
![](../assets/images/esp32-web/wallpapers.jpg)

[Test HTML](https://www.cubicfactory.com/jseditor/)

```bash
WiFi.begin(ssid, password = null, channel = 0, bssid = null, connect = true)
```

- **ssid**, nombre de la red WiFi a la que nos queremos conectar (max 32 char)
- **password**, opcional, contraseña (mínimo 8 char y máximo 64)
- **channel**, opcional, el canal de WiFi a emplear
- **bssid**, opcional, dirección MAC del punto de acceso
- **connect**, indica si queremos conectar inmediatamente (si es false, sólo guarda los parámetros)

```bash

#include <WiFi.h>

const char* ssid     = "ssid";
const char* password = "password";

void setup()
{
  Serial.begin(115200);
  delay(10);
  
  WiFi.mode(WIFI_STA);
  WiFi.begin(ssid, password);
  Serial.print("Conectando a:\t");
  Serial.println(ssid); 


  while (WiFi.status() != WL_CONNECTED) 
  {
    delay(200);
   Serial.print('NO HAY CONEXION');
  }

  // DATOS DE LA RED
  Serial.println();
  Serial.print("Conectado a:\t");
  Serial.println(WiFi.SSID()); 
  Serial.print("IP address:\t");
  Serial.println(WiFi.localIP());
}

void loop() 
{
}

```

La función **WiFi.begin(…)** del ESP guarda los credenciales indicados en la memoria flash no volátil (que se mantiene incluso aunque carguemos otro programa).

Las funciones **WiFi.begin(…)** devuelven el estado del ESP para determinar cuándo hemos establecido la conexión con la red. También podemos obtenerlo en otro momento a lo largo de nuestro programa con la función **WiFi.status()**. Esta variable de estado del ESP puede ser:

|WL_CONNECTED | Conexión establecida |
|--------|--------|
|WL_NO_SSID_AVAIL	|El SSID no se encuentra|
|WL_CONNECT_FAILED	|Contraseña incorrecta|
|WL_IDLE_STATUS	|Wi-Fi está cambiando entre estados|
|WL_DISCONNECTED	|El ESP8266 no está configurado en modo STA|


La función **WiFi.mode(*WIFI_STA*)** es opcional en este ejemplo. Sirve para dejar únicamente el modo **STA** y desactivar el **AP** en caso de que lo hubiéramos configurado previamente ya que, como decíamos, el ESP guarda la configuración del WiFi aunque lo reprogramemos. Las opciones de modos son:

|WIFI_OFF|	Apagado|
|-|-|
|WIFI_STA|	Estacion|
|WIFI_AP|	Access point|
|WIFI_AP_STA|	Station+Acces Point|


Cuando el codigo se hace muy largo es mejor dividirlo en varios ficheros. En este ejemplo se puede crear un fichero dentro del mismo directorio del codigo para almacenar las funciones.

```bash
void ConnectWiFi_STA()
{
   Serial.println("");
   WiFi.mode(WIFI_STA);
   WiFi.begin(ssid, password);
   while (WiFi.status() != WL_CONNECTED) 
   { 
     delay(100);  
     Serial.print('NO HAY CONEXION'); 
   }
 
   Serial.println("");
   Serial.print("Iniciado STA:\t");
   Serial.println(ssid);
   Serial.print("IP address:\t");
   Serial.println(WiFi.localIP());
}
```

Para luego llamarlo dentro del codigo de arduino:

```bash

#include <WiFi.h>
 
#include "data.h"  // tengo definido el ssid y el password
#include "EMA_Utils.hpp"
 
void setup()
{
  Serial.begin(115200);
  
  ConnectWiFi_STA();
}
 
void loop() 
{
}

```

### MULTIPLE REDES WIFI


Existe otra forma de conectarse a una red WiFi, o mejor dicho, a la que tenga mejor señal. Para ello se puede usar la librería **WiFiMulti**.

En la configuración usare **wifiMulti.addAP(…)** para agregar varias redes WiFI proporcionando sus respectivas **SSID** y su **contraseña**. Al arrancar, el ESP se conectará a la red que tenga mejor recepción.

Sin embargo, gestionar un *WiFi múltiple* también supone una sobrecarga de trabajo adicional para el ESP, aunque no demasiado grande. Por lo que usar esta función únicamente cuando realmente sea necesaria. **usarla "porque si".**


```bash

#include <WiFi.h>
#include <WiFiMulti.h>

ESP8266WiFiMulti wifiMulti;

void setup() 
{
  Serial.begin(115200);
  delay(10);
  
  WiFi.mode(WIFI_STA);
  wifiMulti.addAP("ssid 1", "password 1");
  wifiMulti.addAP("ssid 2", "password 2");
  wifiMulti.addAP("ssid 3", "password 3");

  Serial.println("Conectando");
  while (wifiMulti.run() != WL_CONNECTED) 
  {
    delay(250);
    Serial.print('NO HAY CONEXION');
  }

  Serial.println();
  Serial.print("Conectado a:\t");
  Serial.println(WiFi.SSID());
  Serial.print("IP address:\t");
  Serial.println(WiFi.localIP());
}

void loop() 
{
}

```

### FUNCIONES WIFI DEL ESP

Aquí tenemos un resumen de algunas de las funciones adicionales disponibles en ESP WiFi. 

```bash
// Gestión de la conexión
WiFi.reconnect();
WiFi.disconnect(true);
WiFi.isConnected();
WiFi.setAutoConnect(autoConnect);
WiFi.status();

// Devuelven el valor indicado (no sirven para reescribir el valor)
WiFi.SSID();
WiFi.hostname();
WiFi.localIP();
WiFi.subnetMask()
WiFi.gatewayIP();
WiFi.dnsIP(dns_no);
WiFi.macAddress();
```

### ENCENDER LED


```bash
 
#include <WiFi.h>       // Añade ibreria
#define LED 2           // Indica pin que se usará (D2)
#include "data.h"
String p="off";         // dato de apagado del led

// Crea servidor llamado "server" y establece el puerto 80 para acceder
WiFiServer server(80);      

void setup() {
  //Configura D2 como salida y lo pone en un estado bajo (0)
 
  pinMode(LED, OUTPUT);
  
  digitalWrite(LED, LOW);

  //Inicia puerto serial a velocidad de 115200 baudios
  
  Serial.begin(115200);  
  
  Serial.println();
  
  Serial.println("Configurando");

  // Inicia conexión WiFi con las datos de la red especificados
  
  WiFi.begin(ssid1, password1);     //configurando las credenciales


  /**
  Este ciclo imprime un punto tras otro mientras no se establezca conexión.

  **/

  Serial.print("Conectandome");
  while (WiFi.status() != WL_CONNECTED)    // Espera conexión a la red
  {
    delay(500);
    Serial.print(".");                      // Imprime puntos 
  }





  Serial.println();
  Serial.print("Conectado, La dirección IP es: ");
  Serial.println(WiFi.localIP());                    // Imprime la IP del ESP32
 
  server.begin();                                   // Inicia el servidor
  Serial.println("Servidor iniciado");
}

void loop() {


/**

Empezamos creando un cliente al que se nombrará client. Un cliente será cualquier dispositivo de la red que entré a la dirección del ESP32.

**/
  
  WiFiClient client = server.available();   // Recibe las conexciones de clientes


/**

Se crea una condición en la que si el cliente se a conectado se avise mediante el monitor serial. Además se crea una variable llamada currentLine que usare después.

**/

  if (client) {                             // Si hay un cliente
    
    Serial.println("Nuevo cliente.");       // Indica el acceso de un cliente
    
    String currentLine = "";                // Variable para datos de fin de linea



    /**
    
    Usaremos un ciclo while. Mientras el cliente esté conectado (connected) y haya datos (available)
    se guardarán los datos en una variable char llamada c y se imprimiran esos datos leídos del cliente.
    
    **/
    
    while (client.connected()) {            // Cliente conectado
      
      if (client.available()) {             // Datos disponibles para ser leido
      
        char c = client.read();             // Lectura de byte en variable C

        Serial.write(c);                    // Muestra los datos



/**

Se crean unas condiciones en las que se toma en cuenta si el dato leído es un salto de línea (\n)
y la longitud del valor (length) de currentLine es igual a 0 para que se cumplan.

**/
        
        if (c == '\n') {                    // Si el byte es un caracter de nuevo salto de linea
          
          if (currentLine.length() == 0) {    // Si no hay caracteres, entonces lanza el codgo HTML


/**

Si las condiciones anteriores se cumplen se despliega el sitio web desde el ESP32 hacia el cliente en formato HTML.
Para ello se llama a client y se le da la instrucción println para imrpimir el HTML una línea a la vez.

La parte que se está viendo corresponde a los parámetros y cabecera (head) de un documento HTML. Dentro de la cabecera
se indican datos que no se suelen visualizar al navegar en la página. Por ejemplo, los estilos (style) CSS de los botones (.button) que se usarán después.

**/

          
            client.println("HTTP/1.1 200 OK");
            
            client.println("Content-type:text/html");
            
            client.println("Connection: close");
            
            client.println();
            
            client.println("<!DOCTYPE html><html>");
            
            client.println("<head><meta name=\"viewport\" content=\"width=device-width, initial-scale=1\">");
            
            client.println("<link rel=\"icon\" href=\"data:,\">");

            client.println("<style>html{font-family: Helvetica; display: inline-block; margin: 0px auto; text-align: center;}");
            
            client.println(".button {border: none;color: white;padding: 15px 32px; text-align: center;");
            
            client.println("text-decoration: none;display: inline-block;font-size: 16px; margin: 4px 2px;cursor: pointer;}");
            
            client.println(".button1 {background-color: #4CAF50;} /* Green */");
            
            client.println(".button2 {background-color: #008CBA;} /* Blue */");
            
            client.println("</style></head>");
            
            // Creación de botones 



/**


se procede a crear el cuerpo (body) de la página. Se muestra un título usando la etiqueta H1.

Después usando la variable de estado llamada "p" se decide el texto y estilo que tendrá le botón para encender o apagar el led.

Recordemos que la variable tenía el valor “off” y programamos el led para que empiece estando apagado. Por ello se mostrará 
el botón con el texto “ENCENDER” para indicarnos que esa será la acción que podremos realizar.

Siguiendo con el botón “ENCENDER” hay que mencionar que al hacer clic (onClick) sobre él nos llevará a la dirección “/LED-ON”
(location.href). Este cambio será visible en la URL.


**/
            
            client.print("<body><h1>WebServer con ESP32</h1>");

            if(p=="off"){
           
              client.println("<button type='button' class='button button1' onClick=location.href='/LED=ON'> ENCENDER </button><br><br>");                          
            
                }
            
            else{
              
              client.println("<button type='button' class='button button2' onClick=location.href='/LED=OFF'> APAGAR </button><br><br>");                                               
              
              }
         
            client.print("</body></html>");
            client.println();
            
            // Rompe el codigo del while-loop
            
            break;
            
          }





 /**
 
 En caso de que la longitud del valor de la variable currentLine no sea 0 se procede a asignarle un valor vacío.
 
 **/

          
          else {    // Limpiando variable
            
            currentLine = "";
            
          }
          
        }

        else if (c != '\r') {  // Si no hay retorno de carro
          
          currentLine += c;      // agrega al final de la linea
          
        }



   /**
   
   La siguiente sección del código busca dentro de currentLine las direcciones que obtenemos al hacer 
   clic en el botón de la página y modifica el estado del led de acuerdo a ello.

Por ejemplo. Si dentro hallamos “GET /LED=ON” quiere decir que presionamos el botón “ENCENDER”. 
En consecuencia, el pin del led se activará (HIGH) y la variable p cambiará a “on”.

Recuerda que en el código hay una sección que toma en cuenta el valor de p. En este caso al tener “on” 
el texto del botón será “APAGAR” y al hacer clic sobre él nos dirigirá a la dirección “/LED=ON”. Y así sucesivamente mientras se use el botón.
   
   
   **/
               
        // Revisando el datos recibido del url
        
        if (currentLine.indexOf("GET /LED=ON") != -1) { // si esta ok ineOf -> return un valor mayor a 0
        
          digitalWrite(LED, HIGH);               // GET /LED=ON
          
          p="on";
        }
        
        if (currentLine.indexOf("GET /LED=OFF") != -1) {
        
          digitalWrite(LED, LOW);                // GET /LED=OFF
          
          p="off";
        }
      }
    }



/**

Finalmente se cierra la conexión con el cliente.

**/
    // cerrando la conexión
    client.stop();
    Serial.println("Cliente Desconectado");
  }
}


```

