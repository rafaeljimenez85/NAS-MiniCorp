# 3. Virtualización de DMS

A continuación, indicaremos los pasos para realiza la virtualización de NAS **DS3617xs** con un **DMS 6.1-15047** que como anteriormente hemos contado es el Software que tienen los NAS de la marca Synology. La virtualización de este tiene muchos pasos con a continuación detallaremos uno a uno.

## 3.1 Requisitos previos del Laboratorio.

+ Herramienta de Virtualización
	- VMware 12 pro o superior: Podéis descargar la versión de prueba desde [aqui](https://my.vmware.com/en/web/vmware/info/slug/desktop_end_user_computing/vmware_workstation_pro/14_0)
	- VirtualBox (Alternativa): Podéis descarga la herramienta desde [aquí](https://www.virtualbox.org/wiki/Downloads)
+ Imagen de Virtualización
	- Es la imagen que nos dejará arrancar la máquina virtual con si fuera un NAS Synology: [aquí](https://mega.nz/#F!yQpw0YTI!DQqIzUCG2RbBtQ6YieScWg!HA4wSIpI)
+ Sistema Base DMS
	- 6.1-15047: Necesaria para la primera Instalación está disponible en la página oficial de Synology: [aquí](https://archive.synology.com/download/DSM/release/6.1/15047/)
	- 6.1.6-15266: Necesaria para la actualización post-instalación está disponible en la página oficial de Synology: [aquí](https://archive.synology.com/download/DSM/release/6.1.6/15266/)
+ Herramienta de búsqueda de NAS en red
 - Synology Assitant: podéis descargarla desde la página oficial de Synology [aquí](https://www.synology.com/es-es/support/download/DS3617xs#utilities)

## 3.2 Virtualización Step by Step

A continuación, indicare los pasos para la virtualización necesarios para poder arrancar una máquina virtual emulando un NAS DS3617xs

+ Descargar el software indicado en los requisitos previos

+ Crearemos una nueva carpeta en la ruta donde crear las máquinas virtuales de VMware con el nombre que le daremos a la máquina virtual en mi caso `NAS-LAB-1`
	![alt text](/DSM-Virtual/images/paso1.jpg)

+ Dentro la carpeta anterior crearemos una nueva carpeta con el nombre `file` en la que pegaremos todo el software descargado en los requisitos previos
	![alt text](/DSM-Virtual/images/paso2.jpg)

+ Abriremos la herramienta VMware y crearemos una nueva máquina virtual
	![alt text](/DSM-Virtual/images/paso3.jpg)

+ Escogeremos `Custom (advanced)`
	![alt text](/DSM-Virtual/images/paso4.jpg)

+ En este paso es muy importante la compatibilidad de Hardware (Workstation 12.x), en caso de usar la versión 13.x también será necesario.
	![alt text](/DSM-Virtual/images/paso5.jpg)

+ Escogemos la opción de Instalar el SO luego
	![alt text](/DSM-Virtual/images/paso6.jpg)

+ Escogemos que virtualizaremos `Linux` con una versión `Other Linux 3.x kernel 64-bit`
	![alt text](/DSM-Virtual/images/paso7.jpg)

+ Le damos el nombre que deseamos a la máquina virtual y le indicamos donde se virtualizara, tiene que ser la misma en que creamos en el paso 2
	![alt text](/DSM-Virtual/images/paso8.jpg)

+ Escogemos `1 procesador con 2 Cores`
	![alt text](/DSM-Virtual/images/paso9.jpg)

+ Escogemos 2GB de RAM  
    ![alt text](/DSM-Virtual/images/paso10.jpg)

+ Usaremos la red en `modo NAT`  
	![alt text](/DSM-Virtual/images/paso11.jpg)

+ Elegimos el controlador I/O `LSI Logic`  
	![alt text](/DSM-Virtual/images/paso12.jpg)

+ Escogemos el disco de arranque será de tipo `SATA`  
	![alt text](/DSM-Virtual/images/paso13.jpg)

+ Le indicamos que utilizaremos un disco virtual ya existente.  
	![alt text](/DSM-Virtual/images/paso14.jpg)

+ Escogeremos el `synoboot.vmdk` que antes hemos dejado en la carpeta files  
	![alt text](/DSM-Virtual/images/paso15.jpg)

+ Nos preguntara si queremos convertir el disco a un nuevo formato a lo que indicaremos `Keep Existing Format`  
	![alt text](/DSM-Virtual/images/paso16.jpg)

+ Con esto ya hemos terminado la primera parte de la configuración de la máquina virtual  
	![alt text](/DSM-Virtual/images/paso17.jpg)

+ Todavía quedan algunos pasos para poder ejecutar esta máquina virtual y que pueda funcionar así que editaremos las características de la máquina Virtual  
	![alt text](/DSM-Virtual/images/paso18.jpg)

+ Le agregaremos un nuevo disco
	![alt text](/DSM-Virtual/images/paso19.jpg)

+ Tipo SATA  
	![alt text](/DSM-Virtual/images/paso20.jpg)

+ Crearemos un disco nuevo  
	![alt text](/DSM-Virtual/images/paso21.jpg)

+ El disco tiene que tener las siguientes características: 
	- Tamaño `10 GB` o los que consideres
	- El disco tiene que estar asignado por completo `Allocate all disk space now`
	- El disco tiene que estar en un solo fichero `Store virtual disk as a single file`
	![alt text](/DSM-Virtual/images/paso22.jpg)

+ Llamaremos al disco de la manera que consideremos en mi caso `NAS-LAB-1.vmdk` 
	![alt text](/DSM-Virtual/images/paso23.jpg)

+ Hay que tener paciencia ya que VMware tiene que crear un fichero de 10GB ya que se lo indicamos en los pasos anteriores y esto puede tardar unos minutos  
	![alt text](/DSM-Virtual/images/paso24.jpg)

+ Le indicaremos que la unidad de CD que va a usar una imagen ISO que será `DS3617xs 6.1 Jun's Mod V1.02b.iso` que también estará en la carpeta files  
	![alt text](/DSM-Virtual/images/paso25.jpg)

+ Guardamos y ya podemos arrancar nuestra máquina virtual. Una vez aparezca lo que se ve en el siguiente pantallazo podemos abrir la herramienta `Synology Assitant`   
	![alt text](/DSM-Virtual/images/paso27.jpg)

+ Pulsamos el botón de buscar y nos aparecerá nuestro nuevo NAS que como podéis ver está en estado `No instalado`... Vamos a ello.  
	![alt text](/DSM-Virtual/images/paso28.jpg)

+ Pulsamos el botón `Conectar` del `Synology Assitant` que no abrirá el navegador con la URL de nuestro NAS y pulsaremos `Configurar`  
	![alt text](/DSM-Virtual/images/paso29.jpg)

+ Pulsaremos en Examinar para buscar el fichero `DSM_DS3617xs_15047.pat` dentro de la carpeta files que no es nada más ni nada menos que el sistema operativo **DSM 6.1** y pulsaremos `Instalar`  
	![alt text](/DSM-Virtual/images/paso30.jpg)

+ Nos Informara que todos los datos del disco se eliminaran, pero eso no importa porque no tenemos nada!!!  
	![alt text](/DSM-Virtual/images/paso31.jpg)

+ Tras subir el nuevo software el NAS empezara con la instalación
	![alt text](/DSM-Virtual/images/paso32.1.jpg)

+ Tras reiniciarse el sistema ya está actualizado, pero continuaremos con las configuraciones, ahora nos solicita el nombre que tendrá el servidor, un usuario y una contraseña, pero lo importante es que quitéis el check de compartir la ubicación  
	![alt text](/DSM-Virtual/images/paso34.jpg)

+ Lo siguiente es cambiar la configuración de las actualizaciones a descarga manual.
	![alt text](/DSM-Virtual/images/paso35.jpg)

+ Ya que no se trata de una NAS legítimo de Synology no realizaremos el quickConnect por lo cual pulsaremos sobre `Omitir este paso`
	![alt text](/DSM-Virtual/images/paso36.jpg)

+ Desmarca Ayude a mejorar DSM...
	![alt text](/DSM-Virtual/images/paso37.jpg)

+ Y Listo nuestra instalación de DSM sobre VMware ya está finalizada.
	![alt text](/DSM-Virtual/images/paso38.jpg)
