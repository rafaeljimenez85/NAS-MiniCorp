# Hardening de Synology

Como era de esperar, Unos de los pasos más importantes en la configuración de nuestro NAS será segurizarlo para evitar lo máximo posible que nuestro NAS sea víctima de ataques ya que contendrá herramientas muy importantes.

A continuación, os guiare por los pasos básicos para la seguridad de nuestro NAS Synology.

## Actualización a la Ultima Versión de DSM

Como ya sabéis uno de los pasos más importantes para segurizar nuestro NAS será la instalación de la última versión de nuestro OS DSM, que actualmente tras la virtualización está en la versión **6.1-15047** por lo cual necesitamos actualizarlo a la última versión que es la **6.1.6-15266** de para aplicar todos los parches de seguridad y correcciones de Bugs. Para ello seguiremos los siguientes pasos:

+ Lo primero, será descargar desde la página web de oficial de Synology la última versión para nuestro modelo de NAS que es el **DS3617xs** para ello podemos ir a la siguiente página: https://archive.synology.com/download/DSM/release/ donde puedes encontrarlas todas, pero en este caso en específico las URLs para descargar lo necesario:
	- Actualización del OS: https://archive.synology.com/download/DSM/release/6.1.6/15266/DSM_DS3617xs_15266.pat
	- Fichero de comprobación de integridad del fichero anterior: https://archive.synology.com/download/DSM/release/6.1.6/15266/DSM_DS3617xs_15266.pat.md5

![alt text](/DSM-Virtual/images/DownloadDSMupdated.jpg)

+ Lo siguiente será, la comprobación de integridad del fichero **DSM_DS3617xs_15266.pat**. Para lo primero seguiremos los siguientes pasos:
	- Abrir una terminal de windows (cmd) o PowerShell.
	- Irnos a la carpeta donde se encuentran ambos ficheros
	- Ejecutar el siguiente comando: `CertUtil -hashfile <FICHERO> MD5` en este caso en específico `CertUtil -hashfile DSM_DS3617xs_15266.pat MD5`.
	- Ejecutar el siguiente comando: `CertUtil -hashfile DSM_DS3617xs_15266.pat.md5 MD5`.
	- Comparar el resultado obtenido en el 3 paso y en el 4 paso, si el resultado no del hash no es el mismo significa que la integridad del fichero no es correcta.

![alt text](/DSM-Virtual/images/hash.jpg)

+ Ahora iremos directamente a la web de nuestro NAS y abriremos la aplicación `Panel de control`

![alt text](/DSM-Virtual/images/panel%20de%20control.jpg)

+ Iremos al menú `Actualizar y restaurar` y ejecutaremos los siguientes pasos:
	- Pulsaremos el botón `Actualización Manual de DSM`
	- Buscaremos el fichero `DSM_DS3617xs_15266.pat`
	- Pulsaremos `OK`

![alt text](/DSM-Virtual/images/paso51.jpg)

+ Tras cargar el fichero el NAS nos informa que se trata de un proceso largo así que paciencia...

![alt text](/DSM-Virtual/images/paso53.jpg)

+ Tras un reinicio automático del NAS la actualización se ha realizado correctamente.

![alt text](/DSM-Virtual/images/paso54.jpg)

## Cambio de puertos.
Como es habitual en el hardening de un sistema, el cambio de puertos por defecto es una muy buena práctica, por ello vamos a realizar el cambio de puertos de HTTP y HTTPS de nuestro synology para de esta manera dificultar cualquier ataque.

Para realizar el cambio de estos puertos realizaremos los siguientes pasos:

+ En el menú de herramientas de nuestro NAS abriremos el `Panel de Control`

+ Nos dirigimos a `Red` y a la pestaña `Configuración de DSM`

+ En esta pestaña sustituiremos los siguientes valores:
	- **_Puerto de DSM (HTTP):_** del valor 5000 al valor 6969
	- **_Puerto de DSM (HTTPS):_** del Valor 5001 al valor 27001

![imagen](/DSM-Virtual/images/CambioPuertos.jpg)

## Apartado de Seguridad
A continuación, vamos a navegar por el apartado de seguridad dentro del `Panel de control` de synology para habilitar las opciones de seguridad que más beneficien.

![alt text](/DSM-Virtual/images/Seguridad.jpg)

### Pestaña de Seguridad.
En esta pestaña existen diferentes opciones, que detallaremos a continuación

+ **Temporizador de cierre de sesión (minutos):** la sesión de los usuarios se cerrará automáticamente en DSM si permanecen inactivos durante el periodo de tiempo especificado aquí. Introduzca cualquier valor entre 1 y 65535.

+ **Mejorar la compatibilidad del explorador omitiendo la comprobación de IP:** si accede a Synology NAS a través de un proxy HTTP y encuentra desconexiones aleatorias, puede habilitar esta opción para omitir la comprobación de IP. Habilitar esta opción para proporcionar mejor compatibilidad para navegadores detrás de un servidor proxy, pero reducirá el nivel de seguridad. 

+ **Mejorar la protección contra ataques de falsificación de solicitud entre sitios:** esta opción mejora la protección del sistema contra ataques de scripting entre sitios. Esta opción entrará en efecto la próxima vez que inicie una sesión en DSM.

+ **Mejorar la seguridad con el encabezado HTTP de la Directiva de seguridad de contenido (CSP):** esta opción mejora la seguridad del sistema contra ataques de scripting entre sitios (XSS) al permitir solo los datos de fuentes de confianza y restringir la ejecución de comandos en línea.

+ **No permitir que DSM se incruste en iFrame:** puede habilitar esta opción para impedir que otros sitios incrusten DSM en otras páginas web con iFrame, evitando así algunos tipos de ataques desde sitios web malintencionados. Se puede permitir que ciertos sitios web determinados incrusten DSM con iFrame si hace clic en el botón Sitios web permitidos.

+ **Borrar todas las sesiones de inicio de sesión cuando el sistema se reinicie:** todas las sesiones abiertas en el NAS serán cerradas tras un reinicio del mismo, obligando a los usuarios a volver a iniciar sesión nuevamente.

+ **Mostrar notificación del escritorio de DSM cuando la IP actual cambie:** Cuando la IP del usuario conectado actualmente cambia, se envía una notificación de escritorio al usuario.

En mi caso lo he dejado de la siguiente manera:

![alt text](/DSM-Virtual/images/pestanaSeguridad.jpg)

### Pestaña Cortafuegos (Firewall)
En esta pestaña tenemos la opción para habilitar el firewall del Synology que por defecto viene desactivado, pero por supuesto que nosotros habilitaremos y como ya sabemos la primera regla será un `deny all`...

A continuación, daremos os guiare por los pasos a seguir para la correcta configuración inicial, aunque hay que tener en cuenta que más adelante habilitaremos nuevas reglas a medida que avancemos en este laboratorio.

+ Lo primero será activar el check de `Activar Cortafuegos` y `Habilitar Notificaciones del Cortafuegos` esta última nos ayudará a detectar que acciones está realizando el cortafuegos para de esta manera detectar posibles fallos de configuración o ataques.

![alt text](/DSM-Virtual/images/cortafuegos1.jpg)

+ Ahora crearemos 2 nuevos perfiles:
	- CerrarTodo: En este perfil solo tendremos 2 reglas. Habilitado el acceso por HTTPS y todo denegado, esta regla es para usar en caso de emergencia y solo tener acceso a la gestión administrativa del NAS
	- NAS-LAB-1: Es el perfil que usaremos habitualmente y que modificaremos con las nuevas reglas.
	Para ello abriremos el desplegable de `perfil de firewall` y haremos clic en el `+`

![alt text](/DSM-Virtual/images/cortafuegos2.jpg)

+ Escribiremos el nombre del primer perfil `CerrarTodo` y OK

![alt text](/DSM-Virtual/images/cortafuegos3.jpg)

+ Ahora seleccionaremos ese perfil en el desplegable y pulsaremos en `Editar regla`

![alt text](/DSM-Virtual/images/cortafuegos4.jpg)

+ Creamos la primera regla que es denegar todo pulsando el botón `crear`.

![alt text](/DSM-Virtual/images/cortafuegos5.jpg)

+ Creamos la segunda regla que es permitir HTTPS pulsando el botón `crear`. Hay que tener en cuenta que el HTTPS de Administración de la DSM no está en el 443 sino en el **5001** y el HTTP en el 5000

![alt text](/DSM-Virtual/images/cortafuegos6.jpg)

+ Vemos que las reglas ha sido creadas correctamente o no!

![alt text](/DSM-Virtual/images/cortafuegos7.jpg)

+ Efectivamente las reglas **NO** son correctas ya que la primera regla es ***_DENY ALL_***, pero esto se arregla de manera sencilla ya que las reglas son **drag and drop**. Así que las movemos y quedan así:

![alt text](/DSM-Virtual/images/cortafuegos8.jpg)

+ Pulsamos OK

+ Ahora vamos a por el Segundo perfil y el que usaremos habitualmente (NAS-LAB-1). Realizaremos los mismos pasos anteriores, pero también permitiremos por ahora el protocolo HTTP también, aunque más adelante los deshabilitaremos.

+ Las reglas quedan de la siguiente manera

![alt text](/DSM-Virtual/images/cortafuegos9.jpg)

+ En el desplegable verificamos que este la el perfil NAS-LAB-1 y pulsamos `aplicar`

![alt text](/DSM-Virtual/images/cortafuegos10.jpg)

### Pestaña Protección
En esta pestaña tenemos que habilitar la protección DoS (Denegación de servicio) que ayuda a evitar ataques maliciosos a través de Internet.

![alt text](/DSM-Virtual/images/Proteccion1.jpg)

### Pestaña cuenta.
Esta pestaña nos ayudara a proteger las cuentas de ataques externos.

#### 1. Bloqueo automático
La función de bloqueo automático ayuda a mejorar la seguridad de su Synology NAS bloqueando las direcciones IP de clientes con demasiados intentos de conexión fallidos. Esto ayuda a reducir el riesgo de que se fuerce la entrada a cuentas con ataques de fuerza bruta.

También puede crear y administrar una lista de permisos para agregar direcciones de IP de confianza, o una lista de bloqueo para evitar siempre que determinadas direcciones IP inicien sesión.

> Nota:
> Varios servicios y paquetes son compatibles con el bloqueo automático: DSM, SSH, Telnet, rsync, copia de seguridad de red, sincronización de carpetas compartidas, FTP, WebDAV, File Station, Photo Station, Audio Station, Video Station, Download Station, Mail Server, Mail Station, Time Backup, VPN Server, Cloud Station y aplicaciones móviles de Synology.

También hay que tener en cuenta que para este laboratorio no será necesario, pero será muy interesante que a nivel profesional se configuraran correctamente las listas de bloqueos por direcciones IP.

#### 2. Protección de la cuenta
La protección de la cuenta ayuda a mejorar la seguridad de su Synology NAS protegiendo las cuentas de clientes que no sean de confianza con demasiados intentos de conexión fallidos. Esto ayuda a reducir el riesgo de que se fuerza la entrada a cuentas con ataques de fuerza bruta.

> Nota
>Paquetes y servicios compatibles: DSM, File Station, Audio Station, Video Station, Download Station, Mail Station, Cloud Station y aplicaciones móviles de Synology.


Las configuraciones de este este apartado queda de la siguiente forma:

+ **_Habilitar bloqueo automático:_** Marcado como OK
	+ **_Intentos de conexión:_** 5 intentos
	+ **_En (Minutos):_** 10 minutos
	Esto bloqueara la ip de cualquier intento de acceso incorrecto si supera los umbralas de 5 intentos en 10 min

+ **_Habilitar caducidad del bloqueo:_** Sin marcar, en mi caso y al ser un laboratorio no habilito la caducidad del bloqueo, pero es una opción que cada administrador decide si tenerla o no.

+ **_Habilitar protección de las cuentas (Clientes que no son de confianza):_** Marcado como OK. Esto ayuda a reducir el riesgo de que se fuerza la entrada a cuentas con ataques de fuerza bruta.
	+ **_Intentos de conexión:_** 5 intentos
	+ **_En (Minutos):_** 10 minutos
	+ **_Cancelar la protección de la cuenta (Minutos más tarde):_** 120 minutos

+ **_Habilitar protección de las cuentas (Clientes de confianza):_** Marcado como OK. Esto ayuda a reducir el riesgo de que se fuerza la entrada a cuentas con ataques de fuerza bruta.
	+ **_Intentos de conexión:_** 10 intentos
	+ **_En (Minutos):_** 1 minutos
	+ **_Cancelar la protección de la cuenta (Minutos más tarde):_** 30 minutos

![alt text](/DSM-Virtual/images/Cuenta1.jpg)

### Pestaña Certificado
Se puede usar un certificado para proteger los servicios SSL de Synology NAS, como por ejemplo web (todos los servicios HTTPS), correo o FTP. El hecho de tener un certificado permite a los usuarios validar la identidad de un servidor y del administrador antes de enviar cualquier información confidencial.

Mas adelante realizaremos los pasos de creación de un certificado **Let's Encrypt** a la hora de configurar las herramientas.

### Pestaña Avanzado
En esta pestaña podemos configurar lo siguiente:

#### Compresión HTTP
Puede habilitar Compresión HTTP para Synology NAS. Al reducir el ancho de banda del tráfico de red, puede aumentar la velocidad de carga de las páginas web.

#### Perfil TLS/SSL
Puede elegir el nivel de seguridad de las conexiones HTTPS cifradas. Ofrece los 3 niveles siguientes:

+ **_Compatibilidad con versiones actuales:_** solo compatible con los dispositivos móviles y exploradores más recientes.
+ **_Compatibilidad intermedia:_** entre el nivel de seguridad más alto y más bajo. Este es el valor predeterminado del sistema.
+ **_Compatibilidad con versiones anteriores:_** no es seguro, pero es compatible con la mayoría de exploradores y dispositivos móviles.

La configuración de esta pestaña quede de la siguiente manera:

![alt text](/DSM-Virtual/images/Avanzado1.jpg)

## Apartado Red
Por último, en las configuraciones de seguridad habilitaremos las redirecciones http a https para que solo se pueda conectar a través de https a la web del NAS, para ello seguiremos los siguientes pasos

+ Abriremos el `Panel de control`

+ Iremos al apartado de RED y posteriormente a la pestaña de `Configuración de DSM`
![alt text](/DSM-Virtual/images/ConfiguracionDMS1.jpg)

+ Habilitaremos las opciones tal como aparecen en el siguiente pantallazo.
![alt text](/DSM-Virtual/images/ConfiguracionDMS2.jpg)

Tras aplicar estos cambios el servidor web del NAS se reiniciará y ahora, aunque intentemos acceder por HTTP nos redireccionara a HTTPS. Aunque ahora tenemos un problema con el certificado dado que nos indica que no tiene un certificado valido.

### Certificados de navegación.
Se puede usar un certificado para proteger los servicios SSL de Synology NAS, como por ejemplo web (todos los servicios HTTPS), correo o FTP. El hecho de tener un certificado permite a los usuarios validar la identidad de un servidor y del administrador antes de enviar cualquier información confidencial.

Dado que es necesario un dominio para crear los certificados lo que hice fue comprar el dominio **_tripodecorp.black_** en el proveedor OVH que también tiene DDNS (Dinamic Domain Name System) y así podremos acceder a nuestro NAS desde Internet. Vosotros podéis utilizar cualquiera de los soportados pode Synology

Una vez que os creéis la cuenta en OVH o cualquiera de las soportadas tenéis que seguir los siguientes pasos:

+ Abrir el `Panel de Control` he ir a `Acceso Externo` y en la pestaña `DDNS` pulsar el botón `Agregar`

![alt text](/DSM-Virtual/images/DDNS1.jpg)

+ Rellenaremos los datos que nos solicita y le daremos al botón `Probar conexión`

![alt text](/DSM-Virtual/images/DDNS2.jpg)

+ Si el resultado es correcto guardamos los cambios.

Una vez ya tenemos el domino creado ahora nos queda la parte de creación de un certificado de **Let's Encrypt**, para ello seguiremos los siguientes pasos:

+ Nos dirigimos al apartado `Seguridad` dentro de la herramienta `Panel de control`

![imagen](/DSM-Virtual/images/Certificado1.jpg)

+ Luego nos dirigimos a certificados y pulsamos `Agregar` y seleccionamos `Añadir un nuevo certificado`

![imagen](/DSM-Virtual/images/Certificado2.jpg)

+ Seleccionamos Obtener certificado de Let\`s Encrypt

![imagen](/DSM-Virtual/images/Certificado3.jpg)

+ Rellenamos los campos con los siguientes valores:
	- Nombre del dominio: Es el nombre del dominio que acabamos de crear en noip.com (multiservicio.tripodecorp.black)
	- Correo electrónico: correo electrónico donde nos llegaran las notificaciones: (sistemas@tripodecorp.black)

![imagen](/DSM-Virtual/images/Certificado4.jpg)

+ Puede tardar unos minutos en darnos el certificado así que un poco de paciencia

![imagen](/DSM-Virtual/images/Certificado5.jpg)

+ Pasados unos cuantos segundos ya nos otorgó el certificado tal como aparece en el siguiente pantallazo.

![imagen](/DSM-Virtual/images/Certificado6.jpg)