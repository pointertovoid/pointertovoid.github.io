# Virtualizando Ubuntu en Apple Silicon M1

Con la llegada de la familia de procesadores *Apple Silicon*, basados en arquitectura ARM, las posibilidades de ejecutar sobre macOS otros sistemas operativos en una máquina virtual se ha visto muy limitada. Sin embargo, aún es posible llevar a cabo esta tarea con muy buenos resultados. En este artículo analizaremos cómo podemos instalar Ubuntu (o Kubuntu) sobre un Mac con procesador M1 de forma sencilla, rápida y gratuita.

Efectivamente, la llegada de la nueva arquitectura de Apple ha supuesto numerosas ventajas para la plataforma (como la excelente relación potencia de cálculo / consumo eléctrico y la consecuente mejora en la autonomía de los dispositivos), pero también eliminó de un plumazo la posibilidad de *correr* otros sistemas operativos como Windows o Linux de forma virtualizada sobre macOS, debido a la falta de soporte de esta nueva familia de procesadores por parte de "los tres grandes" de la virtualización sobre macOS (VMware, VirtualBox y Parallels), centrados en la arquitectura de Intel.

Si bien es cierto que a la fecha de la redacción de este artículo Parallels ha presentado la [versión 17](https://www.parallels.com/es/products/desktop/whats-new/) de su software de virtualización para Mac que ya *sí* tiene soporte para Apple Silicon y que VMware ha confirmado su intención de [soportar en el futuro](https://blogs.vmware.com/teamfusion/2021/04/fusion-on-apple-silicon-progress-update.html) también la nueva arquitectura de Apple, *aparentemente* la única opción disponible en el presente es la de Parallels que es de pago. Pero, como ya es sabido, con frecuencia *las apariencias engañan*.

## Introducción a UTM para Mac
[UTM para Mac](https://mac.getutm.app) es un *front-end* *[open source](https://github.com/utmapp/UTM)* para el veterano software de virtualización [QEMU](https://www.qemu.org) que permite crear, ejecutar y gestionar máquinas virtuales QEMU de distintas arquitecturas en un *host* macOS con procesador M1 mediante una amigable interfaz gráfica que facilita y simplifica enormemente la tarea de configurar QEMU. Entre las posibilidades que nos ofrece UTM se encuentran las de ejecutar sobre Apple Silicon distribuciones Linux basadas en ARM64, la versión de Windows 10 para ARM e incluso las versines de Windows 10 x86/x64. Veamos cómo instalar la versión ARM64 de Ubuntu en una máquina virtual utilizando UTM.

## Descargando e instalando UTM para Mac
UTM para Mac puede obtenerse de dos maneras diferentes:
1. Desde la [página web](https://mac.getutm.app) oficial de UTM:
   La descarga es gratuita y la instalación es la tradicional en macOS: Abrimos el archivo .dmg descargado y arrastramos la aplicación a una carpeta de nuestra preferencia (como p.e. la carpeta Aplicaciones).
![alt text](https://pointertovoid.github.io/images/2021-09-06-UTM.png "Contenido de UTM.dmg")
2. A través de la [Mac App Store](https://apps.apple.com/es/app/utm-virtual-machines/id1538878817?mt=12): En este caso la aplicación es de pago, pero es idéntica a la que se puede descargar de la página web con la diferencia de que la versión de la App Store admite actualizaciones automáticas. La descarga de la aplicación por esta vía es una forma de contribuir a la financiación del desarrollo del proyecto.
![alt text](https://pointertovoid.github.io/images/2021-09-06-UTM-2.png "UTM en la Mac App Store")