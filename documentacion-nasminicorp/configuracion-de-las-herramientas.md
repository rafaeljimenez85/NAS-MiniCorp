# Configuración de Directory Server

Como hemos visto antes en la esta herramienta este Basada en LDAP3 y nos ayudara tener un control de las cuentas de usuario de nuestra red corporativa. Para configurarlo realizaremos los siguientes pasos:

+ Abriremos la aplicación `Directory Server` en la pestaña `Configuración` configuraremos lo siguiente:
	- **_Activer servidor LDAP:_** Activaremos este check
	- **_FQDN:_** En el campo FQDN (fully qualified domain name), especifique el nombre de dominio de la base de datos LDAP.
	- **_Contraseña:_** Escriba la contraseña de **Bind DN**
	- **_Confirmar contraseña:_** Confirme la contraseña.

![imagen](/DSM-Virtual/images/DirectoryServer1.jpg)

Ahora crearemos los grupos de usuarios del Directory Server, que en realidad nos valdrá sobre todo para llevar la organización del personal, para crear los grupos se segura los siguientes pasos:

+ Iremos a la pestaña `Grupos` y pulsaremos el botón `Crear` en el que rellenaremos los siguientes campos
	- **_Nombre del Grupo:_**
	- **_Descripción de Grupo:_**

![imagen](/DSM-Virtual/images/DirectoryServer6.jpg)

> Nota:
> Los grupos predeterminados del sistema son los siguientes. Tenga en cuenta que no puede crear un grupo con los nombres reservados para uso del sistema.
>+ **_users:_** este es el grupo predeterminado para todos los usuarios.
>+ **_administrators:_** este es el grupo administradores.
>+ **_Directory Operators:_** los usuarios que pertenecen a este grupo tendrán permisos para administrar Directory Server.
>+ **_Directory Consumers:_** los usuarios que pertenecen a este grupo tendrán permisos de lectura para configuraciones y usuarios/grupos en Directory Server. Un servidor Consumer debe pertenecer a este grupo para replicar datos del servidor Provider. Los miembros de este grupo solo deben utilizarse en Bind DN del servidor Consumer y no deben pertenecer a ningún otro grupo. De lo contrario, pueden producirse errores de sincronización debido a permisos incorrectos.
>+ **_Directory Clients:_** los usuarios que pertenecen a este grupo tendrán permisos de lectura para usuarios/grupos en Directory Server. Por motivos de seguridad, se recomienda que se le asigne un usuario en este grupo en caso de que un cliente de LDAP desee unirse a un servidor LDAP.

Ahora lo que tenemos que hacer es crear los usuarios de que van a pertenecer a nuestro entorno para ello haremos lo siguiente.

+ En la pestaña de `Usuarios` pulsaremos en el botón `Crear` lo que nos abrirá una nueva ventana en la que rellenaremos la siguiente información por cada usuario:
	- Nombre
	- Descripción
	- Correo electrónico
	- Contraseña
	- No permitir que el usuario cambie la contraseña: Deshabilitado
	- Deshabilitar la cuenta: Deshabilitado

![imagen](/DSM-Virtual/images/DirectoryServer2.jpg)

+ Al pulsar la pestaña siguiente nos aparecerán los grupos los que puede pertenecer el usuario, seleccionamos los que deseamos.

![imagen](/DSM-Virtual/images/DirectoryServer3.jpg)

+ Al pulsar siguiente nos aparecerá la ventana de `Mas atributos` en la que rellenaremos los campos que deseemos:

![imagen](/DSM-Virtual/images/DirectoryServer4.jpg)

+ La última ventana nos muestra un resumen de la creación del usuario para verificar si los datos son correctos:

![imagen](/DSM-Virtual/images/DirectoryServer5.jpg)

+ Tras la creación del usuario deberemos configurar el resto de opciones que agregaran seguridad a los usuarios. para ello y con el usuario seleccionado nos vamos a la pestaña Avanzado y configurarnos las siguientes opciones:
	- **_Mostrar más información al producirse errores al inicio de sesión:_** Esta opción tiene que estar `DESHAHILITADA` ya que muestra información que puede ayudar a los atacantes a entrar (Solo la podemos activar para pruebas y detección de errores)
	- **_No permitir que los usuarios cambien la configuración personal Salvo la contraseña:_** Esta opción activa
	- **_Forzar el cambio de contraseña por parte del usuario después de que el administrador restablezca la contraseña:_** Esto deberá este activo para que el administrador no sepa las contraseñas de los usuarios por cuestiones de privacidad. Esto no debe ser habilitado si se trata de un usuario de aplicación o genérico.
	- **_Aplicar reglas de fortaleza de contraseña:_** Habilitar esta opción es necesario, ver el pantallazo para más detalles
	- **_Habilitar caducidad de la contraseña:_** Esta opción debe de ser habilitada, ver el pantallazo para más detalle

![imagen](/DSM-Virtual/images/DirectoryServer7.jpg)

+ Tras las configuraciones avanzadas de las cuentas, solo nos queda fortalecer un poco más las cuentas con las reglas de bloque, para ello seguiremos los siguientes pasos:
	- Ir a la pestaña Bloqueo automático y habilitar las siguientes opciones:
	- **_Habilitar bloqueo automático:_** Habilitar esta opción
	- **_Intentos de conexión:_** 5 intentos 
	- **_En (Minutos):_** 5 minutos 
	- **_Habilitar caducidad del bloqueo:_** Habilitar esta opción 
	- **_Desbloquear tras (minutos):_** 30 minutos

![imagen](/DSM-Virtual/images/DirectoryServer8.jpg)

Los usuarios que he creado para este son los siguientes:

![imagen](/DSM-Virtual/images/DirectoryServer9.jpg)


## Configuración de LDAP en el NAS

+ Ahora necesitamos conectar nuestro NAS al LDAP para poder usar estos usuarios por ejemplo para la configuración de usuarios accederán a la VPN. Para ello necesitaremos abrir el `Panel de control` y dirigirnos al apartado `Dominio\\LDAP`

+ Posteriormente vamos a ir a la pestaña `LDAP` y configuraremos los datos de la aplicación `Directory Server`

![imagen](/DSM-Virtual/images/LDAPconfiguration1.jpg)

+ Al pulsar sobre `Aplicar` no solicitara el Usuario y contraseña del Administrador de LDAP al que anteriormente hemos tenido que editar para cambiarle la password por defecto.

![imagen](/DSM-Virtual/images/LDAPconfiguration2.jpg)

+ Tras este paso nos aparecerá el mensaje:

![imagen](/DSM-Virtual/images/LDAPconfiguration3.jpg)

+ Ahora podemos ver que los usuarios del LDAP ya aparecen reflejados en la pestaña `Usuario de LDAP` al igual que los grupos en la pestaña `Grupo de LDAP`

![imagen](/DSM-Virtual/images/LDAPconfiguration4.jpg)



# Configuración del Servidor NTP

Podemos configurar nuestro sistema con servidor NTP (Network Time Protocol), para que así todo nuestro entorno tenga la misma hora, para ello realizaremos los siguientes pasos:

+ Abriremos la aplicación de `Panel de control` 

+ Vamos al apartado `Opciones Regionales`

+ Vamos a la pestaña `Servicio NTP` y activamos la propiedad.


![imagen](/DSM-Virtual/images/NTP1.jpg)


# Configuración de Proxy Server
Para la configuración del Proxy Server realizaremos los siguientes pasos:

+ Abriremos la aplicación desde el botón de aplicaciones del NAS
+ En la pestaña `Configuración` realizaremos configuraremos los siguiente:
	- **_Correo electrónico del gerente del servidor proxy:_** Dirección de correo electrónico del administrador o administradores.
	- **_Puerto del proxy web:_** Puerto predefinido del servidor proxy (3128), lo cambiaremos por seguridad y utilizaremos el 8989 
	- **_Habilitar almacenamiento en caché:_** Habilita el almacenamiento en caché del servidor proxy
	- **_Habilitar inicio de sesión:_** Permitirá que el servidor proxy pueda registrar cada solicitud de HTTP. Los registros del servidor proxy se almacenan en `/var/packages/ProxyServer/target/squid/var/logs/`.

![imagen](/DSM-Virtual/images/proxyServer1.jpg)
 
 + Ahora en la pestaña de `Cache` Configuraremos los siguientes valores:

![imagen](/DSM-Virtual/images/proxyServer2.jpg)

 + Ahora en la pestaña de `Control de acceso` Configuraremos los siguientes valores:

![imagen](/DSM-Virtual/images/proxyServer3.jpg)

+ Ahora en la pestaña de `Autenticación` Configuraremos los siguientes valores: 

![imagen](/DSM-Virtual/images/proxyServer4.jpg)

+ Ahora en la pestaña de `Implementación proxy` Configuraremos los siguientes valores:

![imagen](/DSM-Virtual/images/proxyServer5.jpg)

+ Tras pulsar aplicar el NAS nos indica que es necesario habilitar el Servidor DHCP que se ampliara en el siguiente punto.





# Configuración del Servidor DHCP

Vamos a configurar nuestro NAS con Servidor DHCP para que de esta manera funciona correctamente la autodetección de proxy, para ello vamos a realizar los siguientes pasos.

+ En el menú de aplicación abriremos el `Panel de control` y nos dirigiremos al `DHCP Server`

+ Nos mostrara las Tarjetas de Red que posee nuestro NAS y vamos a configurar la única que tiene llamada `LAN 1` para ello pulsaremos `Editar`

![imagen](/DSM-Virtual/images/DHCPServer1.jpg)

+ En la pestaña `DHCP Server` configuraremos los siguiente:

![imagen](/DSM-Virtual/images/DHCPServer2.jpg)

+ Antes de darle a OK tenemos que crear una lista de subred a la que el Servidor DHCP dará IPs, para ello pulsamos en `Agregar` y ponemos los datos que deseamos en mi caso lo siguiente:
	- **_IP Inicio:_** 192.168.119.100
	- **_IP Final:_** 192.168.119.200
	- **_Mascara:_** 255.255.255.0
	-**_Pasarela:_** 192.168.119.2 (Ip del Router)
	- **_Opciones DHCP:_** ntp-server = IP de hora.roa.es (1192.168.119.176)
	- **_Opciones DHCP:_** smtp-server = IP Local (192.168.119.176)

![imagen](/DSM-Virtual/images/DHCPServer3.jpg)


# Configuración de VPN Server

Ahora vamos a habilitar el servicio de VPN para que nuestros empleados puedan entrar vía VPN a la red y de esta manera evitar que se instalen herramientas de acceso remoto. Para ello vamos a realizar los siguientes pasos:

+ Abrir `VPN Server` desde el menú de aplicación del NAS

+ Nos vamos a la pestaña `Configuración` y rellenamos los campos con los siguientes datos:
	- **_Interface de red:_** LAN 1
	- **_Tipo de Cuenta:_** Usuarios LDAP
	- **_Otorgar permisos de VPN a los usuarios recién añadidos:_** No, por seguridad nosotros elegiremos únicamente los usuarios que tendrán acceso a la VPN.

![imagen](/DSM-Virtual/images/VPNServer1.jpg)

+ Vamos a la pestaña `L2TP/IPSec` y usaremos la siguiente configuración:
	- **_Habilitar el servidor VPN L2TP/IPSec:_** Activado
	- **_Dirección IP dinámica:_** 192.16.0.0
	- **_Número máximo de conexiones:_** Establezca el Número máximo de conexiones para limitar el número de conexiones VPN simultáneas.
	- **_Número máximo de conexiones con la misma cuenta:_** Establezca el Número máximo de conexiones con la misma cuenta para limitar el número de conexiones VPN simultáneas con la misma cuenta.
 	- **_Autenticación:_** Elija cualquiera de las siguientes opciones del menú desplegable Autenticación para autenticar los clientes VPN: 
	- **_MTU:_** Establezca la MTU (Unidad máxima de transmisión) para limitar el tamaño del paquete de datos transmitido a través de VPN.
	- **_Utilice DNS manual:_** Marque Utilice DNS manual y especifique la dirección IP de un servidor DNS para llevar DNS a clientes de L2TP/IPSec. Si se inhabilita esta opción, el servidor DNS utilizado por Synology NAS se enviará a los clientes.
	- **_Clave compartida:_** Introduzca y confirme una clave compartida anteriormente. Esta clave secreta se debe dar a los usuarios de L2TP/IPSec VPN para autenticar la conexión
	- **_Habilitar modo compatible con SHA2-256 (96 bit):_** Marque Habilitar modo compatible con SHA2-256 (96 bit) para permitir que ciertos clientes (no RFC estándar) usen la conexión L2TP/IPSec.

![imagen](/DSM-Virtual/images/VPNServer2.jpg)

+ Nos vamos a la pestaña `Privilegios` y decidimos que usuarios tendrán acceso a qué tipo de VPN, En este caso los usuarios son los que hemos creado anteriormente y solo con privilegios para acceder a L2TP/IPSec

![imagen](/DSM-Virtual/images/VPNServer3.jpg)


# Configuración del servidor Radius

Lo siguiente será configurar nuestro servidor RADIUS para poder utilizar de esta manera WPA2-Enterprise en nuestra Wifi, para ello realizaremos los siguientes pasos:

+ Abriremos la aplicación `Radius Server` desde el menú de aplicaciones del NAS

+ En la pestaña configuración rellenaremos los siguientes datos:

![imagen](/DSM-Virtual/images/RadiusServer1.jpg)

## Configuración de clientes Radius

La página de Clientes contiene una lista de clientes, como por ejemplo enrutadores inalámbricos o servidores VPN, a los que se permite realizar solicitudes al RADIUS Server de autenticaciones, autorizaciones y contabilidad. Para cada cliente deberá introducir una dirección IP, junto con una secret key, que es una cadena de texto utilizada como contraseña entre los clientes RADIUS y el RADIUS Server. Las claves secretas se deben configurar tanto en el cliente como en el RADIUS Server.

Crearemos como cliente nuestro router para de esta manera poder utilizar los beneficios que nos da el uso del servidor RADIUS. Para ello seguiremos los siguientes pasos

+ Iremos a la pestaña `Clientes` y daremos el botón agregar

![imagen](/DSM-Virtual/images/RadiusServer2.jpg)

+ Rellenaremos los datos solicitados de la siguiente manera:

![imagen](/DSM-Virtual/images/RadiusServer3.jpg)

+ Esta es la imagen final de los clientes del servidor RADIUS

![imagen](/DSM-Virtual/images/RadiusServer4.jpg)

> **Métodos de autenticación compatibles con RADIUS Server:**
> PAP, MS-CHAP, PEAP, EAP-MSCHAPv2, EAP-TTLS



# Configuración de MailPlus Server

Vamos a configurar el servidor de correos de la empresa con MailPlus Server, y para ello seguiremos los siguientes pasos:

+ Abriremos la aplicación desde el menú de aplicaciones del NASA

+ Inmediatamente se nos abrirá el asistente para guiarnos en los pasos para la creación del servidor

+ Seleccionamos `Crear un nuevo sistema de correo`

![imagen](/DSM-Virtual/images/MailPlusServer1.jpg)

+ Rellenaremos los siguientes datos:

![imagen](/DSM-Virtual/images/MailPlusServer2.jpg)

+ Nos mostrara un resumen

![imagen](/DSM-Virtual/images/MailPlusServer3.jpg)

+ Procede a la configuración 

![imagen](/DSM-Virtual/images/MailPlusServer4.jpg)

+ Termina la configuración

![imagen](/DSM-Virtual/images/MailPlusServer5.jpg)

## Configuración de Servicios

Ahora vamos a configurar los servicios del MailPlus Server:

+ Vamos a la pestaña servicios y dejaremos la configuración de la siguiente manera

![imagen](/DSM-Virtual/images/MailPlusServer6.jpg)


## Configuraciones de Seguridad

Ahora configuraremos todos los apartados de Seguridad.

+ La pestaña de `Correo no deseado` tiene que quedar de la siguiente manera:

![imagen](/DSM-Virtual/images/MailPlusServer7.jpg)

+ La pestaña de `Antivirus` tiene que quedar de la siguiente manera:

![imagen](/DSM-Virtual/images/MailPlusServer8.jpg)

+ La pestaña de `Autenticación` tiene que quedar de la siguiente manera:

![imagen](/DSM-Virtual/images/MailPlusServer9.jpg)


## Configuración de Cuentas

Aquí habilitaremos las cuentas que se necesitan para el correo. La aplicación solos nos deja gestionar 5 cuentas de correo, en caso de necesitar más será necesario la adquisición de una licencia que podéis consultar aquí: https://www.synology.com/es-es/products/MailPlus_License

![imagen](/DSM-Virtual/images/MailPlusServer10.jpg)

## Configuración del Portal de aplicaciones.

Para configurar correctamente el acceso al Mail Plus necesitamos configurar el `Portal de aplicaciones` para ellos realizaremos lo siguiente:

+ Abrimos `Panel de Control` y nos vamos al apartado `Portal de aplicaciones`

+ Seleccionamos MailPlus y pulsamos editar

![imagen](/DSM-Virtual/images/MailPlusPA1.jpg)

+ Dejaremos la siguiente configuración y guardamos.

![imagen](/DSM-Virtual/images/MailPlusPA2.jpg)

Tras realizar estos pasos podremos acceder al Webmail a través de la URL creada en el `Portal de aplicaciones`

![imagen](/DSM-Virtual/images/MailPlusPA3.jpg)

Solo nos queda logarnos con nuestro usuario de LDAP et voilà!

![imagen](/DSM-Virtual/images/MailPlusPA4.jpg)


# Configuración de Chat

También instalaremos la aplicación de chat de Synology, servirá para acelerar las comunicaciones internas. Para la instalación seguiremos los siguientes pasos:

+ Abrir la aplicación `Centro de paquetes` e instalar la aplicación

![imagen](/DSM-Virtual/images/Chat1.jpg)

+ Tras la instalación de la aplicación se crearán 2 nuevas herramientas `Consola de administración de chat` y `chat` 
	- **_Consola de administración de chat:_** Esta nos permitirá configurar las utilidades del chat
	- **_Chat:_** Es un enlace directo a la aplicación de chat

![imagen](/DSM-Virtual/images/Chat3.jpg)

+ La configuración de la aplicación `Consola de administración de chat` será la siguiente:

![imagen](/DSM-Virtual/images/Chat2.jpg)

+ Con esto, la configuración del chat ya estaría realizada, ahora tenemos que configurar el `Portal de aplicación` para ello abrimos `Panel de Control` y nos vamos al apartado `Portal de aplicaciones`

+ Seleccionamos chat y pulsamos editar

![imagen](/DSM-Virtual/images/Chat4.jpg)

+ Dejaremos la siguiente configuración y guardamos.

![imagen](/DSM-Virtual/images/Chat5.jpg)

Tras realizar estos pasos podremos acceder al chat a través de la URL creada en el `Portal de aplicaciones`

![imagen](/DSM-Virtual/images/Chat6.jpg)

Solo nos queda logarnos con nuestro usuario de LDAP et voilà!

![imagen](/DSM-Virtual/images/Chat7.jpg)



# Web Station.

Con un host virtual puede configurar varios sitios web en Synology NAS. Se requiere un nombre de dominio para la configuración del host virtual. Cada host virtual puede ser basado en nombre o basado en puerto. Cada sitio web de host virtual requiere un directorio de raíz del documento para el acceso y almacenamiento de archivos.

Lo que haremos en poner la página web de la empresa en esta Web Station. Para ello seguiremos los siguientes pasos:

+ Instalaremos la aplicación `Web Station` desde el `Centro de Paquetes`

+ Abriremos la aplicación `File Station` desde el menú de herramientas

![imagen](/DSM-Virtual/images/WebStation1.jpg)

+ Iremos a la carpeta `web` y ponemos en ella el index.html de nuestra web y el contenido necesario para que funcione en mi caso realice lo siguiente:
	- Renombre el index.html que está en la carpeta `web` por index.html.back (Para poder reusarla en caso necesario)
	- Mediante Drag and Drop subí el nuevo `Index.html` y las carpetas `assets` e `images` quedando el directorio de la siguiente manera:

![imagen](/DSM-Virtual/images/WebStation2.jpg)

> **NOTA**
> **_Hay que tener extremado cuidado de NO borrar el fichero wpad.dat ya que sino nuestro Proxy no funcionara correctamente._**

Ahora podemos ver nuestra nueva web desde la URL de nuestro NAS

![imagen](/DSM-Virtual/images/WebStation3.jpg)