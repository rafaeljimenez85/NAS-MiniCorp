# 5. Hardening de Synology

Como era de esperar, Unos de los pasos más importantes en la configuración de nuestro NAS será segurizarlo para evitar lo máximo posible que nuestro NAS sea víctima de ataques ya que contendrá herramientas muy importantes.

A continuación, os guiare por los pasos básicos para la seguridad de nuestro NAS Synology.

## 5.1. Actualización a la Ultima Versión de DSM

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

## 5.2. Cambio de puertos.
Como es habitual en el hardening de un sistema, el cambio de puertos por defecto es una muy buena práctica, por ello vamos a realizar el cambio de puertos de HTTP y HTTPS de nuestro synology para de esta manera dificultar cualquier ataque.

Para realizar el cambio de estos puertos realizaremos los siguientes pasos:

+ En el menú de herramientas de nuestro NAS abriremos el `Panel de Control`

+ Nos dirigimos a `Red` y a la pestaña `Configuración de DSM`

+ En esta pestaña sustituiremos los siguientes valores:
	- **_Puerto de DSM (HTTP):_** del valor 5000 al valor 6969
	- **_Puerto de DSM (HTTPS):_** del Valor 5001 al valor 27001

![imagen](/DSM-Virtual/images/CambioPuertos.jpg)

## 5.3. Apartado de Seguridad
A continuación, vamos a navegar por el apartado de seguridad dentro del `Panel de control` de synology para habilitar las opciones de seguridad que más beneficien.

![alt text](/DSM-Virtual/images/Seguridad.jpg)

### 5.3.1 Pestaña de Seguridad.
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

### 5.3.2 Pestaña Cortafuegos (Firewall)
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

### 5.3.4. Pestaña Protección
En esta pestaña tenemos que habilitar la protección DoS (Denegación de servicio) que ayuda a evitar ataques maliciosos a través de Internet.

![alt text](/DSM-Virtual/images/Proteccion1.jpg)

### 5.3.5. Pestaña cuenta.
Esta pestaña nos ayudara a proteger las cuentas de ataques externos.

#### 5.3.5.1. Bloqueo automático
La función de bloqueo automático ayuda a mejorar la seguridad de su Synology NAS bloqueando las direcciones IP de clientes con demasiados intentos de conexión fallidos. Esto ayuda a reducir el riesgo de que se fuerce la entrada a cuentas con ataques de fuerza bruta.

También puede crear y administrar una lista de permisos para agregar direcciones de IP de confianza, o una lista de bloqueo para evitar siempre que determinadas direcciones IP inicien sesión.

> Nota:
> Varios servicios y paquetes son compatibles con el bloqueo automático: DSM, SSH, Telnet, rsync, copia de seguridad de red, sincronización de carpetas compartidas, FTP, WebDAV, File Station, Photo Station, Audio Station, Video Station, Download Station, Mail Server, Mail Station, Time Backup, VPN Server, Cloud Station y aplicaciones móviles de Synology.

También hay que tener en cuenta que para este laboratorio no será necesario, pero será muy interesante que a nivel profesional se configuraran correctamente las listas de bloqueos por direcciones IP.

#### 5.3.5.2. Protección de la cuenta
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

### 5.3.6. Pestaña Certificado
Se puede usar un certificado para proteger los servicios SSL de Synology NAS, como por ejemplo web (todos los servicios HTTPS), correo o FTP. El hecho de tener un certificado permite a los usuarios validar la identidad de un servidor y del administrador antes de enviar cualquier información confidencial.

Mas adelante realizaremos los pasos de creación de un certificado **Let's Encrypt** a la hora de configurar las herramientas.

### 5.3.7. Pestaña Avanzado
En esta pestaña podemos configurar lo siguiente:

#### 5.3.7.1. Compresión HTTP
Puede habilitar Compresión HTTP para Synology NAS. Al reducir el ancho de banda del tráfico de red, puede aumentar la velocidad de carga de las páginas web.

#### 5.3.7..2 Perfil TLS/SSL
Puede elegir el nivel de seguridad de las conexiones HTTPS cifradas. Ofrece los 3 niveles siguientes:

+ **_Compatibilidad con versiones actuales:_** solo compatible con los dispositivos móviles y exploradores más recientes.
+ **_Compatibilidad intermedia:_** entre el nivel de seguridad más alto y más bajo. Este es el valor predeterminado del sistema.
+ **_Compatibilidad con versiones anteriores:_** no es seguro, pero es compatible con la mayoría de exploradores y dispositivos móviles.

La configuración de esta pestaña quede de la siguiente manera:

![alt text](/DSM-Virtual/images/Avanzado1.jpg)

## 5.4. Apartado Red

### 5.4.1 Interfaz de Red
En este apartado gestionaremos todas las configuraciones relacionadas con la interfaz de red, en este caso en específico la interfaz de red ´LAN´

+ Abriremos el `Panel de control`

+ Iremos al apartado de RED y posteriormente a la pestaña de `Interfaz de red`

+ Seleccionaremos la interface `LAN 1` y pulsaremos en editar
![alt text](/DSM-Virtual/images/LAN1.jpg)

+ Iremos a la pestaña `IPv6` 

+ En el desplegable `Configuración IPv6` seleccionaremos desactivado ya que no vamos a realizar uso de IPv6
![alt text](/DSM-Virtual/images/LAN2.jpg)


### 5.4.2 Configuración de DSM
Por último, en las configuraciones de seguridad habilitaremos las redirecciones http a https para que solo se pueda conectar a través de https a la web del NAS, para ello seguiremos los siguientes pasos

+ Abriremos el `Panel de control`

+ Iremos al apartado de RED y posteriormente a la pestaña de `Configuración de DSM`
![alt text](/DSM-Virtual/images/ConfiguracionDMS1.jpg)

+ Habilitaremos las opciones tal como aparecen en el siguiente pantallazo.
![alt text](/DSM-Virtual/images/ConfiguracionDMS2.jpg)

Tras aplicar estos cambios el servidor web del NAS se reiniciará y ahora, aunque intentemos acceder por HTTP nos redireccionara a HTTPS. Aunque ahora tenemos un problema con el certificado dado que nos indica que no tiene un certificado valido.

### 5.4.1. Certificados de navegación.
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

## 5.5 Deshabilitar servicios innecesarios

Es necesario deshabilitar servicios que no utilizaremos para disminuir lo máximo posible la superficie de ataque. y para ello seguiremos los siguientes pasos:

+ Abriremos el `Panel de control` y iremos al apartado `Terminal y SNMP`

+ Iremos a la pestaña `Terminal` y deshabilitaremos el acceso ssh y telnet a nuestro dispositivo.
![imagen](/DSM-Virtual/images/Terminal1.jpg)

> **NOTA:** En caso de ser necesario un acceso a terminal se desaconseja el uso de telnet y el caso de habilitar ssh tener en cuenta dos consejos
> + Cambiar el puerto por defecto de ssh
> + Crear una regla en el firewall específica para ssh y así evitar el acceso al mismo desde internet 

+ Ahora, iremos a la pestaña `SNMP` y deshabilitaremos el SNMP.
![imagen](/DSM-Virtual/images/SNMP1.jpg)

> **NOTA:** En caso de ser necesario un acceso a SNMP hay tener en cuenta dos consejos
> + No utilizar SNMP con versión inferior a V3
> + Crear una regla en el firewall específica para SNMP y así evitar el acceso al mismo desde internet

Tras realizar estos pasos, continuamos con el proceso para deshabilitar servicios innecesarios.

+ Vamos al apartado `Servicios de Archivos`

+ Vamos la pestaña `SMB/AFP/NFS` donde deshabilitaremos todos los servicios ya que en nuestro caso no utilizaremos ninguno.
![imagen](/DSM-Virtual/images/SMB1.jpg)

+ Vamos a la pestaña `FTP` y al igual que en el caso anterior deshabilitamos los servicios de FTP, FTPS y SFTP
![imagen](/DSM-Virtual/images/FTP1.jpg)

+ Vamos a la pestaña `TFTP` y al igual que en el caso anterior deshabilitamos el servicio.
![imagen](/DSM-Virtual/images/TFTP1.jpg)

+ Vamos a la pestaña `rsync` y al igual que en el caso anterior deshabilitamos el servicio.
![imagen](/DSM-Virtual/images/rsync1.jpg)

+ Vamos a la pestaña `Avanzado` y al igual que en el caso anterior deshabilitamos los servicios.
![imagen](/DSM-Virtual/images/Avanzado2.jpg)

## 5.6 Notificaciones

Aunque no forma parte específica del hardening de un sistema incluimos las notificaciones del sistema, para poder estar informado en todo momento de cualquier anomalía del sistema y poder tener una pronta reacción ante cualquier aviso. Par configurar las notificaciones realizaremos los siguientes pasos:

### 5.6.1 Notificación por correo

+ Abrimos el `Panel de control` y nos vamos al apartado Notificaciones

+ Vamos a la pestaña `Correo Electrónico`, en el configuraremos una cuenta alternativa a la de nuestra micro empresa, ya que como veremos más adelante, nuestro servidor de correo estará en este mismo NAS y en caso de fallo del sistema no enviaría el correo.

+ En este caso configuramos una cuenta de Gmail para que realice el envió y la misma cuenta para que reciba estos correos. En la configuración de nuestra cuenta alternativa tenemos establecido un reenvió de los correos a nuestra cuenta principal así que en caso de un funcionamiento correcto no necesitaremos estar pendiente de la cuenta alternativa.
![imagen](/DSM-Virtual/images/Notificacion1.jpg)

+ Realizamos una prueba de envió y verificamos que está configurado correctamente ya que nos llega a los 2 buzones de correo.
![imagen](/DSM-Virtual/images/Notificacion2.jpg)
![imagen](/DSM-Virtual/images/Notificacion3.jpg)

### 5.6.2 Notificación por SMS

+ Lo primero será abrirse una cuenta en www.clickatell.com para poder utilizar el envío de SMS desde esa plataforma.
![imagen](/DSM-Virtual/images/Clickatell.jpg)

+ Vamos a la pestaña `SMS` y configuramos los datos de nuestra cuenta de clickatell
![imagen](/DSM-Virtual/images/Clickatell.jpg)

+ Lo único necesario para el envío de SMS será pagar los paquetes de SMS que no son muy caros aproximadamente 200 SMS por 10 euros

### 5.6.3 Notificaciones Push
Esta opción nos permitirá que nos envié correos desde el servidor central de Synology y también el envío de notificaciones push a nuestros dispositivos móviles y navegadores.

+ Para utilizar la función de envío de correos mediante el servidor de Synology y el envío de notificaciones push necesitas crearte una cuenta synology en el apartado `QuickConenect` --> `General` --> `Iniciar sesión o registrar una cuenta Synology`

+ Para sincronizar tu móvil con el NAS para recibir notificaciones push será necesario descargarte la app `DS Finder` y logarte con ella en el NAS y activar las notificaciones

![imagen](/DSM-Virtual/images/push4.jpg)

+ Para recibir notificaciones push mediante el navegador necesitas instalar un plugin en el mismo, este plugin puedes conseguirlo presionando el botón `Habilitar las notificaciones del explorador` en la pestaña de push dentro de `Panel de Control`

![imagen](/DSM-Virtual/images/push7.jpg)

La configuración final queda de la siguiente manera:

![imagen](/DSM-Virtual/images/push1.jpg)

### 5.6.4 Avanzado

En esta pestaña encontrareis todas las notificaciones que desde el sistema DSM pueden ser enviada por los tres métodos anteriores, hay que tener en cuenta que se puede editar cualquier mensaje y otros factores a tener en cuenta es que los envíos de SMS y Push solo envían el texto del asunto, pero por correo electrónico envía el contenido.

![imagen](/DSM-Virtual/images/Avanzado3.jpg)

## 5.7. Cifrado de carpetas y gestión de claves de cifrado.

Como parte del Hardening también veremos la creación de carpetas cifradas usando el almacén de claves y para ello es fundamental tener un dispositivo de almacenamiento USB conectado a nuestra NAS y seguir los siguientes pasos:

### 5.7.1. Almacén de claves

+ Lo primero será conectar una unidad de almacenamiento USB al nuestro NAS y formatear dicha unidad para proceder a utilizarla. para ello nos vamos al `Panel de control` al apartado `Dispositivos Externos` y pestaña `Dispositivos Externos`, aquí veremos que tenemos un USB conectado a nuestro NAS

![imagen](/DSM-Virtual/images/USB1.jpg)

+ Procedemos al formateo de la unidad USB pulsando el botón `Formatear` y seleccionamos los siguientes valores.

![imagen](/DSM-Virtual/images/USB2.jpg)

+ Tras Formatear la Unidad USB procederemos a crear el Administrador de claves y para ello nos vamos al apartado `Carpeta Compartida` y buscamos dentro de `Acción` la opción de `Administrador de claves`

![imagen](/DSM-Virtual/images/claves1.jpg)

+ Rellenaremos los siguientes valores

![imagen](/DSM-Virtual/images/claves2.jpg)

+ Nos pedirá confirmación de la contraseña:

![imagen](/DSM-Virtual/images/claves3.jpg)

Una vez realizado estos paso el administrador de claves ya estará creado, por lo cual lo siguiente será crear una nueva carpeta para cifrarla y comprobar su funcionamiento. Para ello seguiremos los siguientes pasos:

+ Seleccionábamos el botón `Crear` en el apartado `Carpetas Compartidas` y crearemos la carpeta `Cifrada` para realizar las pruebas.

![imagen](/DSM-Virtual/images/cifrada1.jpg)

+ Ahora seleccionaremos las opciones de cifrado y administrador de claves.

![imagen](/DSM-Virtual/images/cifrada2.jpg)

> **Nota:** Existen 2 maneras de cifrar la carpeta:
> + **_Frase de contraseña:_** Cualquiera que conozca la frase de contraseña puede eliminar el cifrado de las claves mediante este método.
> + **_Clave de equipo:_** Tan solo el Synology NAS vinculado puede eliminar el cifrado de las claves cifradas mediante una clave de equipo.

+ También habilitaremos la opción de `integridad de datos avanzada`

![imagen](/DSM-Virtual/images/cifrada3.jpg)

> **_Habilitar la suma de comprobación para la integridad de datos avanzada:_** para proteger una carpeta compartida con estrategias de suma de comprobación CRC32 y copia en escritura, puede habilitar la suma de comprobación de datos para la integración de datos avanzada durante la creación de la carpeta compartida. Se aplica una suma de comprobación de CRC32 para verificar si los datos siguen siendo idénticos a los grabados originalmente y si el sistema utiliza automáticamente redundancia RAID para corregir los datos dañados. La copia en escritura ayuda a mejorar la consistencia de datos cuando se produce un cierre anómalo del sistema.

+ Nos indica un aviso que tenemos que aceptar para cifrar la carpeta:

![imagen](/DSM-Virtual/images/cifrada4.jpg)

+ Comprobamos que nuestros usuarios del LDAP tenga acceso de escritura y lectura.

![imagen](/DSM-Virtual/images/cifrada5.jpg)

+ Verificamos que la nueva carpeta esta creada con un candado.

![imagen](/DSM-Virtual/images/cifrada6.jpg)

+ Por último comprobamos que en administrador de claves está reflejado nuestra carpeta.

![imagen](/DSM-Virtual/images/cifrada7.jpg)
