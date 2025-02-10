
# Instalación de Mac OS en AMD Ryzen (VMWare) con Opencore - Rendimiento Mejorado - Compatible con Sequoia / Sonoma / etc. | AMD OS X - Potenciando tu Mac con Innovación AMD

Escribí este tutorial para ayudar a los usuarios de AMD a crear su propia versión de macOS en VMWare, donde creo que el sistema funciona de manera más fluida (ver ítem 8).

\*\*\* Primero que todo, no realices ningún cambio en el archivo .vmx de VMWare. Se recomienda crear una nueva máquina virtual desde cero para que se recree este archivo por defecto y sin cambios.

\*\*\* Utiliza la versión de VMWare Workstation Pro proporcionada en el enlace del tutorial. (Si la instalas, tendrás que aplicar un nuevo Patch según se describe).

1) Primero debes descargar VMWare desde el sitio oficial e instalarlo eligiendo la versión gratuita para uso no comercial:

Otras descargas:

\- VM Workstation Pro 17.5.2 (Probado y también se puede usar. Es gratis para uso personal).

\- Descargar Unlocker para parchear VMWare:

![mega.nz](https://forum.amd-osx.com/proxy.php?image=https%3A%2F%2Fmega.nz%2Frich-file.png&hash=00cce83a3df297351d3f4adb724bb732&return_error=1)

\- Nueva versión de unlocker probada en VMWare Workstation Pro.

\- Después de instalar VMWare, ciérralo.

Extrae todos los archivos de unlocker en una carpeta, abre el símbolo del sistema (terminal) en modo administrador y ejecuta "win-install.cmd".

Este parche libera la opción de configuración de la versión Apple Mac OS X en VMWare en Configuración de Máquina Virtual, Opciones, Apple Mac OS X:

Usa "macOS 14" al configurar tu máquina virtual:

[![1697595264886.png](https://forum.amd-osx.com/data/attachments/12/12179-03bcbea227da6aa5a3ba383c974c28bb.jpg "1697595264886.png")](https://forum.amd-osx.com/attachments/1697595264886-png.12124/)

2) También descarga una ISO de macOS Sonoma 14.0 (probado):  
Nota: Todas las imágenes ISO son Vanilla.

Enlace1:

Enlace2:

![mega.nz](https://forum.amd-osx.com/proxy.php?image=https%3A%2F%2Fmega.nz%2Frich-file.png&hash=00cce83a3df297351d3f4adb724bb732&return_error=1)

Creo que otras versiones anteriores de macOS pueden funcionar ya que en este proyecto se utilizó opencore 0.95. Aún no lo he probado con otras.

\----------------------------------------  
O Descargar

Sonoma 14.5 (probado).

Enlace1:

Enlace2:

![mega.nz](https://forum.amd-osx.com/proxy.php?image=https%3A%2F%2Fmega.nz%2Frich-file.png&hash=00cce83a3df297351d3f4adb724bb732&return_error=1)

\----------------------------------------  
O Descargar

Nueva versión Sonoma 14.6.1 (probado).

Enlace1:

[https://fastupload.io/b2b892fd799bc081](https://fastupload.io/b2b892fd799bc081)

\----------------------------------------  
O Descargar Para fines de prueba, también estoy subiendo una ISO que contiene **macOS Sequoia Beta 2**, compatible con Opencore 1.0.0:

Enlace1:

Enlace2:

![mega.nz](https://forum.amd-osx.com/proxy.php?image=https%3A%2F%2Fmega.nz%2Frich-file.png&hash=00cce83a3df297351d3f4adb724bb732&return_error=1)

Nota:  
**La Beta de Sequoia aún no funciona con servicios de inicio de sesión de Apple como Apple Store, Cloud y otras aplicaciones que requieren inicio de sesión en máquinas virtuales que ejecutan Windows como host.  
Si deseas utilizar Sequoia con servicios de inicio de sesión, necesitas instalar Sonoma 14.5 y hacer una actualización de sistema a Sequoia.**

3) Descarga un pequeño archivo de imagen de disco VMWare:

Editar:

Esta versión solo funciona con procesadores Ryzen con 4 núcleos o más  
Descargar Nueva Versión de archivo de imagen de disco VMWare Aquí: OPENCORE 1.0.0 - 4 núcleos

Este archivo contiene Opencore 1.0.0 en una partición EFI y **debe ser seleccionado como imagen de disco en la unidad SATA 0:0 en la configuración de VMWare**. Siempre debe ser utilizado de esta manera.

O DESCARGAR:

Esta versión solo funciona con procesadores Ryzen con 8 núcleos o más  
Descargar Nueva Versión de archivo de imagen de disco VMWare Aquí: OPENCORE 1.0.1 - 8 núcleos

Este archivo contiene Opencore 1.0.1 en una partición EFI y **debe ser seleccionado como imagen de disco en la unidad SATA 0:0 en la configuración de VMWare**. Siempre debe ser utilizado de esta manera.

O DESCARGAR:

Esta versión solo funciona con procesadores Ryzen con 16 núcleos o más  
Descargar Nueva Versión de archivo de imagen de disco VMWare Aquí: OPENCORE 1.0.1 - 16 núcleos

Este archivo contiene Opencore 1.0.1 en una partición EFI y **debe ser seleccionado como imagen de disco en la unidad SATA 0:0 en la configuración de VMWare**. Siempre debe ser utilizado de esta manera.

Configuración de VMWare:

1) Memoria: 8GB

2) Procesadores:

4 núcleos (si estás usando la imagen de disco de VMWare con 4 núcleos)  
8 núcleos (si estás usando la imagen de disco de VMWare con 8 núcleos)  
16 núcleos (si estás usando la imagen de disco de VMWare con 16 núcleos)

[![1697563518529.png](https://forum.amd-osx.com/data/attachments/12/12165-8bdee92a1c27a18824140aaaeb6044b9.jpg "1697563518529.png")](https://forum.amd-osx.com/attachments/1697563518529-png.12110/)

3) Disco duro (Sata): macOS\_Opencore\_1.0\_4\_cores.vmdk o macOS\_Opencore\_1.0.1\_8\_cores.vmdk o macOS\_Opencore\_1.0.1\_16\_cores.vmdk (SATA 0:0) siempre debe ser la primera unidad de arranque.  
En las versiones más recientes de VMWare, elige "Mantener formato existente" si te lo solicitan al agregar la imagen de esta unidad.

[![1718427341446.png](https://forum.amd-osx.com/data/attachments/14/14544-9ac88503596a90b112d6c3b86cebece3.jpg "1718427341446.png")](https://forum.amd-osx.com/attachments/1718427341446-png.14254/)

4) Disco Duro 2 (Sata): Esta unidad es donde se instalará macOS. Cualquier tamaño que consideres adecuado.

5) CD/DVD (SATA): Usa archivo de imagen ISO (Sonoma.iso)

[![1697563742532.png](https://forum.amd-osx.com/data/attachments/12/12167-8b7d6330da909197c20dca1d69206135.jpg "1697563742532.png")](https://forum.amd-osx.com/attachments/1697563742532-png.12112/)

6) Controlador USB 3.1 (probado)

[![1697563943198.png](https://forum.amd-osx.com/data/attachments/12/12168-8b7d6330da909197c20dca1d69206135.jpg "1697563943198.png")](https://forum.amd-osx.com/attachments/1697563943198-png.12113/)

7) Adaptador de Red: Puente Automático.

Durante la instalación, solo debes borrar o formatear la unidad de instalación de macOS que creaste usando la Utilidad de Discos. Nunca lo hagas en la unidad OPENCORE (debe permanecer intacta):

[![1697564233230.png](https://forum.amd-osx.com/data/attachments/12/12169-01fdcb27a72525ac8c95a6aabc586913.jpg "1697564233230.png")](https://forum.amd-osx.com/attachments/1697564233230-png.12114/)

[![1697564292473.png](https://forum.amd-osx.com/data/attachments/12/12170-a93070f686df5b618cfef6c7b381cc01.jpg "1697564292473.png")
