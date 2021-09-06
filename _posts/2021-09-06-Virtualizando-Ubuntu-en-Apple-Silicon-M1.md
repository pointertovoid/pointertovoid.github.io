---
layout: post
title: Virtualizando Ubuntu en Apple Silicon M1
---

Con la llegada de la familia de procesadores *Apple Silicon*, basados en arquitectura ARM, las posibilidades de ejecutar sobre macOS otros sistemas operativos en una máquina virtual se ha visto muy limitada. Sin embargo, aún es posible llevar a cabo esta tarea con muy buenos resultados. En este artículo analizamos cómo podemos instalar Ubuntu sobre un Mac con procesador M1 de forma sencilla, rápida y gratuita.

Efectivamente, la llegada de la nueva arquitectura de Apple supuso numerosas ventajas para la plataforma (como la excelente relación potencia de cálculo / consumo eléctrico y la consecuente mejora en la autonomía de los dispositivos), pero también eliminó de un plumazo la posibilidad de *correr* otros sistemas operativos como Windows o Linux de forma virtualizada sobre macOS, debido a la falta de soporte de esta nueva familia de procesadores por parte de "los tres grandes" de la virtualización sobre macOS (VMware, VirtualBox y Parallels), centrados en la arquitectura de Intel.

Si bien es cierto que a la fecha de la redacción de este artículo Parallels ha presentado la [versión 17](https://www.parallels.com/es/products/desktop/whats-new/) de su software de virtualización para Mac que ya *sí* tiene soporte para Apple Silicon y que VMware ha confirmado su intención de [soportar en el futuro](https://blogs.vmware.com/teamfusion/2021/04/fusion-on-apple-silicon-progress-update.html) también la nueva arquitectura de Apple, *aparentemente* la única opción disponible en el presente es la de Parallels que es de pago. Pero, como ya es sabido, con frecuencia *las apariencias engañan*.

## Introducción a UTM para Mac
[UTM para Mac](https://mac.getutm.app) es un *front-end* *[open source](https://github.com/utmapp/UTM)* para el veterano software de virtualización [QEMU](https://www.qemu.org) que permite crear, ejecutar y gestionar máquinas virtuales QEMU de distintas arquitecturas en un *host* macOS con procesador M1 mediante una amigable interfaz gráfica que facilita y simplifica enormemente la tarea de configurar QEMU. Entre las posibilidades que nos ofrece UTM se encuentran las de ejecutar sobre Apple Silicon distribuciones Linux basadas en ARM64, la versión de Windows 10 para ARM e incluso las versiones de Windows 10 x86/x64. Veamos cómo instalar la versión ARM64 de Ubuntu en una máquina virtual utilizando UTM.

## Descargando e instalando UTM para Mac
UTM para Mac puede obtenerse de dos maneras diferentes:
1. Desde la [página web](https://mac.getutm.app) oficial de UTM:
   La descarga es gratuita y la instalación es la tradicional en macOS: Abrimos el archivo .dmg descargado y arrastramos la aplicación a una carpeta de nuestra preferencia (como p.e. la carpeta Aplicaciones).
![alt text](https://pointertovoid.github.io/images/2021-09-06-UTM.png "Contenido de UTM.dmg")
2. A través de la [Mac App Store](https://apps.apple.com/es/app/utm-virtual-machines/id1538878817?mt=12): En este caso la aplicación es de pago, pero es idéntica a la que se puede descargar de la página web con la diferencia de que la versión de la App Store admite actualizaciones automáticas. La descarga de la aplicación por esta vía es una forma de contribuir a la financiación del desarrollo del proyecto.
![alt text](https://pointertovoid.github.io/images/2021-09-06-UTM-2.png "UTM en la Mac App Store")

## Creación y configuración de la máquina virtual
Los pasos para conseguir tener Ubuntu corriendo en una máquina virtual creada con UTM son los siguientes:
1. Descargamos la ISO de Ubuntu Server para ARM desde la [página oficial](https://ubuntu.com/download/server/arm). En el momento de la escritura de este artículo las dos versiones disponibles son la 20.04.3 LTS y la 21.04 (descargaremos una u otra dependiendo de que prefiramos la última versión con soporte extendido o simplemente la versión más reciente).
2. Abrimos UTM y pulsamos en *Create a New Virtual Machine*.
3. En la ventana que nos aparece, en la pestaña *Information* introducimos un nombre para nuestra máquina virtual y, opcionalmente, seleccionamos un icono para la misma.
![alt text](https://pointertovoid.github.io/images/2021-09-06-UTM-3.png "Nombramos nuestra máquina virtual")
4. En la pestaña *System* indicamos *ARM64 (aarch64) en *Architecture* y seleccionamos la cantidad de memoria RAM que queremos reservar para nuestra máquina virtual (en mi caso he elegido 4 GB, la mitad de la RAM que tiene mi MacBook).
![alt text](https://pointertovoid.github.io/images/2021-09-06-UTM-4.png "Configuramos la arquitectura y la cantidad de RAM")
5. Configuramos dos unidades de disco virtuales en la pestaña *Drives*: Una del tipo *Disk Image* e interfaz *VirtIO* a la que hemos asignado 10 GB de espacio y que hará el papel de disco duro principal de nuestra máquina virtual, y otra del tipo *CD/DVD (ISO) Image* e interfaz *USB* que hará el papel de unidad de CD/DVD. Esta última la marcaremos como *Removable Drive*. 
![alt text](https://pointertovoid.github.io/images/2021-09-06-UTM-5.png "Configuración de las unidades de disco virtuales de la máquina virtual")
6. A continuación, en la pestaña *Sharing* activaremos *Enable Clipboard Sharing* para que podamos compartir el portapapeles entre macOS y el sistema operativo de la máquina virtual una vez que hayamos terminado de configurarla (en particular, una vez que hayamos instalado las *SPICE guest agent tools* en la máquina virtual, como veremos en breve).
![alt text](https://pointertovoid.github.io/images/2021-09-06-UTM-6.png "Configuración del uso compartido del portapapeles")
7. Finalmente pulsamos el botón *Save* para guardar nuestra configuración y en la pantalla principal de nuestra máquina virtual seleccionamos la imagen ISO de Ubuntu Server que descargamos anteriormente.
![alt text](https://pointertovoid.github.io/images/2021-09-06-UTM-7.png "Seleccionamos la ISO de Ubuntu")

## Arranque de la máquina virtual e instalación de Ubuntu
Una vez configurada la máquina virtual, la arrancaremos pulsando el botón de *Play* y se iniciará el instalador de Ubuntu, que configuraremos de la forma convencional (las opciones por defecto que el instalador nos ofrecerá en los distintos pasos son las adecuadas para la mayoría de las instalaciones). Cuando haya terminado la instalación, tendremos ante nosotros la consola de Ubuntu Server ejecutándose; procedermos a "extraer" el ISO del instalador de Ubuntu y a reiniciar la máquina virtual. Ambas acciones las llevaremos a cabo pulsando los botones correspondientes que hay en la barra de herramientas de la ventana de UTM.

## Últimos retoques e instalación (opcional) de un entorno de escritorio
La versión Server de Ubuntu por defecto no incluye ningún servidor gráfico ni ningún entorno de escritorio. Para instalar uno (así como para configurar otros bloques de funciones de Ubuntu Server de forma fácil) es muy útil la herramienta `tasksel` que habremos de instalar previamente:
```bash
$ sudo apt install tasksel
```
A continuación elegiremos el entorno de escritorio que queremos instalar:
- Para Gnome:
```bash
$ sudo tasksel install ubuntu-desktop
```
- Para KDE:
```bash
$ sudo tasksel install kubuntu-desktop
```
> Por algún motivo `tasksel` suele fallar a la hora de instalar los entornos de escritorio. Esta situación se resuelve volviendo a ejecutar `tasksel` de nuevo.

Ya podemos reiniciar nuestra máquina virtual:
```bash
$ sudo reboot
```
Y tendremos nuestro entorno de escritorio funcionando (KDE en nuestro caso):
![alt text](https://pointertovoid.github.io/images/2021-09-06-UTM-8.png "Ubuntu con escritorio KDE en nuestra máquina virtual")
El último paso será instalar las *SPICE guest agent tools* para poder utilizar el portapapeles compartido y también un directorio compartido (que se mostrará como un servidor WebDAV en `http://127.0.0.1:9843/`), mediante el siguiente comando (que también se muestra en la captura anterior):
```bash
$ sudo apt install spice-vdagent spice-webdavd
```
> Si queremos utilizar la funcionalidad de directorio compartido, habremos de activarla en la pestaña *Sharing* de la configuración de nuestra máquina virtual.

Ya sólo quedará ajustar en KDE la resolución de pantalla que queramos utilizar e instalar los paquetes de software que queramos, según el uso que vayamos a hacer del sistema operativo que acabos de instalar.
## Conclusión
Como hemos podido comprobar, la instalación de Ubuntu en una máquina virtual de QEMU ha sido una tarea muy sencilla y directa gracias al uso de *UTM para Mac*, una aplicación bastante desconocida en general y que, sin embargo, no tiene nada que envidiar a otras plataformas de virtualización comerciales en cuanto a sencillez de uso e incluso a rendimiento (esto último es más cierto en el caso de máquinas virtuales de Linux; en el caso de crear una máquina virtual para instalar Windows 10 ARM, ahí sí hay mucha diferencia en cuanto a estabilidad, prestaciones y rendimiento respecto a otras soluciones comerciales como Parallels Desktop).