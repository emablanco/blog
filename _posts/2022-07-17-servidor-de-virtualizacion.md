---
layout: single

title: Creando mi Servidor de Virtualización - QEMU/KVM 

excerpt: "**Un Servidor de Virtualización**

es el proceso de dividir un servidor fícico en múltiples servidores virtuales y aislados por medio de una aplicación 
de software. Cada servidor virtual puede ejecutar sus propios sistemas 
operativos de manera independiente"


date: 2022-07-17

classes: wide

header:

    teaser: /assets/images/debian.png

    teaser_home_page: true
    
    icon: /assets/images/logofairy.png

categories:

    - Debian
    - QEMU
    - KVM
    - libvirt


tags:  

- Servidor de Virtualización
---

![](/assets/images/kvm/kvm.png)

## Introducción

Un hipervisor es una plataforma que permite aplicar diversas técnicas de 
control de virtualización para utilizar al mismo tiempo diferentes sistemas 
operativos en una misma computadora.

Para comprobar si la CPU soporta la virtualización y si se encuentra activa. 
El siguiente comando sirve para CPU Intel como AMD. Si el comando devuelve un
valor mayor a 0 es compatible con el proceso:

```bash
egrep -c '(vmx|svm)' /proc/cpuinfo 	
```
/proc/cpuinfo es el fichero del sistema de ficheros virtual /proc que da acceso mediante "ficheros" a las estructuras de datos del núcleo de Linux; cpuinfo lista las características de la CPU y vmx es el flag que se usa para indicar que el procesador usa esta tecnología; smd es el flag para AMD-V. egrep busca líneas de un fichero que contengan la expresión regular indicada, y si aparecen los flags listará la línea completa. Si no lista nada, entonces es que el procesador no tiene esa funcionalidad o está desactivada
	
Otra manera de controlar si el CPU soporta virtualización es:

```bash
apt-get install cpu-checker
	
/usr/sbin/kvm-ok
```
Si el resultado es:

    INFO: /dev/kvm exists\par
	KVM acceleration can be used

## KVM

	
KVM es una solución de virtualización para Linux en hardware x86 que contiene extensiones de virtualización (Intel VT o AMD-V). KVM forma parte del Kernel de Linux desde la versión 2.6.20. Específicamente, con KVM podemos convertir a Linux en un hipervisor para que nuestro host ejecute entornos virtuales, es decir, máquinas virtuales. Cada máquina virtual se implementa como un proceso regular de Linux
		
KVM en general es un módulo del kernel que permite el uso de tecnologías de extensiones de virtualización de Intel o AMD. Estas extensiones permiten que múltiples sistemas operativos compartan una CPU física sin interferir entre ellos. Por otro lado, no resuelven compartir todos los dispositivos de hardware, para esto KVM requiere una lógica extra y aquí es dónde comenzamos a hablar de QEMU.


	
## QEMU
	
QEMU es un emulador de procesadores basado en la traducción dinámica de binarios, realiza la conversión del código binario de la arquitectura del CPU físico, en código entendible por la arquitectura del CPU virtual de la VM.
		
El resultado de usar QEMU es poder ejecutar el código escrito para una arquitectura de procesador en una arquitectura completamente diferente.
		
QEMU es capaz de emular varias plataformas de hardware diferentes. Además del hardware básico de estos sistemas, QEMU también proporciona emulación de varios módulos adicionales, como tarjetas gráficas, tarjetas de sonido, controladores, dispositivos serie/paralelo/USB ,etc. Esto significa que las computadoras pueden ser completa y totalmente emuladas.
		
## QEMU/KVM
	
En el hardware real, el sistema operativo traduce las instrucciones de los programas para que sean ejecutadas por el CPU físico. En una máquina virtual (el CPU está virtualizado por el hipervisor) el hipervisor tiene que traducir las instrucciones del CPU virtual y convertirlo en instrucciones para el CPU físico. Esta traducción tiene una gran sobrecarga de rendimiento.
		
Para minimizar esta sobrecarga, los procesadores admiten extensiones de virtualización. Intel soporta una tecnología llamada **VT-X** y el equivalente AMD es **AMD-V**. Con el uso de estos, una parte de CPU físico se puede asignar directamente al CPU virtual. Así que las instrucciones de la CPU virtual se pueden ejecutar directamente en la parte del CPU físico. Evitando así la traducción que tendría que hacer el hipervisor.
		
KVM es el módulo del kernel de Linux que permite esta asignación de CPU físico para CPU virtual. Esta asignación proporciona la aceleración de hardware para la máquina virtual y aumenta su rendimiento. QEMU utiliza esta aceleración cuando el tipo de virtualización es elegido como KVM.
		
Los desarrolladores de KVM aprovecharon la arquitectura QEMU, así las llamadas al sistema pasan por el módulo KVM, mientras que QEMU se utiliza para emular los dispositivos. Entonces al trabajar juntos, KVM accede directamente al CPU físico y a la memoria, a su vez QEMU emula los recursos de hardware, como el disco duro, video, USB, etc.
			
		
**Conclusión**

KVM es un módulo del kernel de Linux para la explotación de extensiones VT y necesita a QEMU para la funcionalidad completa como hipervisor. En cambio QEMU es autosuficiente y solo necesita a KVM para aumentar su rendimiento. Por otro lado KVM por sí solo no puede proporcionar la solución de virtualización completa, necesita a QEMU.
	
## libvirt QEMU/KVM}
	
libvirt es una API de código abierto, un demonio y una herramienta de gestión para gestionar la virtualización de la plataforma. Se puede utilizar para administrar KVM, Xen, VMware ESXi, QEMU y otras tecnologías de virtualización.
	
El paquete libvirt proporciona el demonio libvirtd que maneja las llamadas a la biblioteca, administra las máquinas virtuales y controla el hipervisor.
	
## Instalación
		
En Debian es completamente fácil crear un servidor de virtualización, solo con ejecutar esta linea de comandos se instalaran las herramientas necesarias.

```bash
apt install qemu qemu-kvm qemu-system qemu-utils libvirt-clients libvirt-daemon-system virtinst virt-manager bridge-utils
```
## Configuración de Red
		
Una vez que finaliza la instalación ya tendremos el servidor de virtualización funcionando. Ahora solo queda realizar la configuración de red, donde se debe crear una interfaz **br0** que sera un puente con la interfa fisica. Con esta interfaz se logra que las máquinas virtuales obtengan IPs del mismo rango que las PC de la red lan, en lugar de obtener IP de una red virtual.

```bash
			
nvim /etc/network/interfaces                                                    

```
Para la creación de una interfaz virtual se debe editar el archivo _/etc/network/interfaces_ agregando las siguientes lineas:

```bash

				
The primary network interface                                                 

allow-hotplug eth1                                                              
iface eth1 inet static                                                          
auto br0                                                                        
iface br0 inet static                                                           
		address 192.168.1.222                                                   
		netmask 255.255.255.0                                                   
		gateway 192.168.1.1                                                     
		bridge_ports eth1                                                       
		up /usr/sbin/brctl std br0 on 

```

## Virt Manager

Para la creación de maquinas virtuales utilizare \textbf{virt-manager}. El cual es una aplicación de interfaz gráfica para crear máquinas virtuales a través de libvirt.
	
Mediante el protocolo **SSH** junto con el parámetro **-X** se proporciona la capacidad de tunelizar el tráfico X11 a través de la conexión \textbf{SSH} para permitir conexiones gráficas remotas.
	
```bash
ssh root@192.168.1.222 -X

```
Una vez dentro del servidor se debe llamar al **virt-manager** para dentro de nuestro ordenador se visualice la interfaz gráfica de este programa:
	
```bash
virt-manager

```
Listo, ahora se puede administrar el servidor de virtualización de forma grafica:

![](/assets/images/kvm/1.png)

- [Wikipedir](https://en.wikipedia.org/wiki/Libvirt)
- [KVM y QEMU](https://galvarado.com.mx/post/kvmqemu)
- [VT-x-Intel AMD-V](https://github.com/JJ/IV/blob/master/documentos/temas/Intro_concepto_y_soporte_fisico.md)
    
 
