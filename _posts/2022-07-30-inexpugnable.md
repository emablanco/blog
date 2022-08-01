---
layout: single

title: Servidor Inexpugnable - GNU/Linux

excerpt: "**Debido a la información sensible que maneja una** 
empresa, sean secretos industriales, información de clientes, etc. Cualquier medida de seguridad que surja en torno a los mismos, es necesaria para poder garantizar la integridad y el buen funcionamiento. En este caso, cifrar los discos duros es una de las formas más adecuadas para protegerlos y que se encuentren seguros de cara a que terceros puedan acceder a ellos sin nuestro permiso y obtener información privilegiada.

En este caso explicare como crear un Servidor con RAID, LVM y Cifrado AES. Esto permitirá que toda la información secreta que se almacene, a la cual nadie debe tener acceso, se mantenga segura con tan solo un reboot. 


Acá no hace falta destruir los discos duros o borrar la información con todo el tiempo que eso conlleva, solo basta con apagar el equipo y nadie que no tenga la contraseña del RAID podrá acceder al sistema.
"


date: 2022-07-30

classes: wide

header:

    teaser: /assets/images/seguro/logo.png

    teaser_home_page: true
    
    icon: /assets/images/logofairy.png

categories:

    - Inexpugnable
    - Linux
    - Raid
    - lvm
    - AES

tags:  

- Maquina Inexpugnable
---

Si la información que almacenas en tu servidor es importante y deseas que en caso de robo, secuestro, extravío, auditoría policial, etc. Les sea imposible de acceder y que solo tu puedas descifrar los discos duros para recuperar la información. Te presento esta guía de buenas practicas, para crear un servidor que no solo cifra los disco duros, sino que los cifra sobre arreglos. Cifrar sobre arreglos significa que se puede optar por conservar la información con tolerancia a fallo o destrucción al mas mínimo toque.

- **RAID 0:** Garantiza que al mas mínimo cambio todo el disco quede inútil. Haciendo imposible poder recuperar la información de cualquier otro disco duro.

 Si alguien esta tratando de abrir el servidor y robar un disco 		duro, al momento que quite un disco la información se destruirá. 	Suponiendo que se logre llevar todos los discos, debe conseguir 		la contraseña de todo el RAID para poder ensamblar el sistema
operativo.


- **RAID 1,3,5,6:** Estos RAID o cualquier otra combinación están pensadas para proteger la información, con copias espejos o con paridad. 

Con tan solo un reboot o un shutdown y nadie podrá acceder a sistema operativo y mucho menos a la información.


## QEMU/KVM

Como siempre todo lo realizo en mi servidor de Virtualización. Donde creare una maquina virtual con EFI y el sistema operativo sera **Debian**.

![](/assets/images/inexpugnable/qemu.png)

Por razones obvias omitiré el proceso de instalación de Debían, ya que si estas leyendo esta guía sabrás como hacer una instalación de un sistema operativo. Solo me centrare en la gestión de los discos duros.

En la siguiente imagen se puede observar que el servidor cuenta con cinco disco duros.

![](/assets/images/inexpugnable/1.png)

Hay que elegir que tipo de raid se va a usar. dependiendo del tipo de información que se almacene se debe seleccionar el RAID. como asumo que la informacion que 
se almacena no sera delito, el raid que usare sera un raid con tolerancia a fallos..

![](/assets/images/inexpugnable/2.png)

se debe separar el directorio donde se almacenaran los kernel y una que es donde se
almacenara los archivos del EFI. con 100MB bastara..
    
Preparar los discos para hacer un raid
![](/assets/images/inexpugnable/3.png)


![](/assets/images/inexpugnable/4.png)

Elegir el raid, yo seleccionare un raid 5

![](/assets/images/inexpugnable/5.png)

seleccionar los discos raid
![](/assets/images/inexpugnable/6.png)

Terminar Proceso


![](/assets/images/inexpugnable/7.png)

Cifrar el raid 5 

![](/assets/images/inexpugnable/8.png)

aes
![](/assets/images/inexpugnable/9.png)

finalizar 

![](/assets/images/inexpugnable/10.png)

Configurar el cifrado

![](/assets/images/inexpugnable/11.png)

Crear volumen cifrado 
![](/assets/images/inexpugnable/12.png)

Seleccionar volumen

![](/assets/images/inexpugnable/13.png)
Ingresar Password 
![](/assets/images/inexpugnable/14.png)
Confirmar pasword
![](/assets/images/inexpugnable/15.png)
Ahora al volumen cifrado hay que decirle que es un lvm

![](/assets/images/inexpugnable/16.png)
Configurar LVM

![](/assets/images/inexpugnable/17.png)


Crear Volumenes


![](/assets/images/inexpugnable/19.png)

Crear sistema de archivo sobre los Volumenes

![](/assets/images/inexpugnable/20.png)
Instalar sistema base 

![](/assets/images/inexpugnable/21.png)
Selecionar lo que se desea instalar , como es un servidor no se necesita un 
entorno grafico. queda en cada quien que es lo que se instalara

![](/assets/images/inexpugnable/22.png)
Finalizo la instalacion
![](/assets/images/inexpugnable/23.png)
una vez que se reinicia el sistema cargara el grub, dado que el directorio boot
no se encuentra cifrado. una vez que el grupo carge se solicitara el password 
del cifrado . 
![](/assets/images/inexpugnable/24.png)
Una vez que se inicia el sistema se sincronisan los discos del raid
![](/assets/images/inexpugnable/25.png)
Informacion de los discos 
![](/assets/images/inexpugnable/26.png)
informacion de los discos
![](/assets/images/inexpugnable/27.png)
informacion del grupo de volumen y los volumenes logicos 




Es una técnica conocida como cold boot attack (ataque de arranque en frío ), utilizada en informática forense, en procedimientos de recuperación de datos e inevitablemente en actividades de piratería ilegal.

Aprovecha una característica física de los bancos de RAM, en particular de las DRAM, que por su propia naturaleza no pierden datos de inmediato ante una fallo repentino en la alimentación, sino que siguen un tipo de descarga que es mucho más lenta cuanto más fríos estén los componentes electrónicos.

En pocas palabras, la RAM pierde su contenido en un tiempo proporcional a la temperatura.


[RAM](https://www.kicksecure.com/wiki/Cold_Boot_Attack_Defense)
[Congelar RAM](https://es.quora.com/Es-cierto-que-puede-extraer-datos-de-la-RAM-de-una-computadora-congel%C3%A1ndola-C%C3%B3mo-se-hace)
En condiciones normales y de estado estacionario, hablamos de unos pocos milisegundos, pero si se enfría por debajo de -50 ° C (por ejemplo, a través de un spray como los disponible en el mercado [1]), el tiempo de descarga puede incluso llegar a unas pocas decenas de segundos.

Ese tiempo es suficiente para desconectarlo de la placa base, colocarlo en otra PC y leer su contenido a través de una herramienta adecuada que lo vuelca.

Recordar que, excepto en casos excepcionales, a) la RAM nunca está encriptada a diferencia del sistema de archivos, b) que a menudo contiene temporalmente las claves utilizadas por los sistemas de seguridad y c) que todo lo que usted está haciendo se coloca momentáneamente allí (como lo que actualmente estoy escribiendo aquí en Quora antes de publicar el contenido), está claro que, como técnica, aunque requiere acceso físico a la computadora, es particularmente intrigante no solamente para actividades completamente legítimas.


