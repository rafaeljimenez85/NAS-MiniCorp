# 6. Instalación de herramientas de Micro Empresa.

Tras realizar los pasos del hardening del sistema realizaremos la instalación de las herramientas que usaremos para prestar servicio y seguridad a la micro empresa. El listado de herramientas que instalaremos es el siguiente:


+ **_Proxy Server:_** El servidor proxy es una aplicación que proporciona memoria en caché y control de accesos para dispositivos basados en HTTP. Mientras almacena en caché puede reservar espacio de almacenamiento en su Synology NAS para guardar contenido web. Cuando el usuario realiza una solicitud HTTP, el servidor proxy recupera contenido de la memoria cache de su Synology NAS, y así se consigue acelerar los servicios y ahorrar tiempo. También puede utilizar el servidor proxy para impedir que usuarios accedan a páginas web restringidas y así mantener su red segura y productividad.

+ **_RADIUS Server:_** El Servicio de autenticación remota telefónica de usuario (RADIUS) es un protocolo de red que ofrece de forma centralizada autenticación, autorización y gestión de cuentas de usuario para el acceso a una red inalámbrica o con cable. El sistema local DiskStation, Active Directory y el servidor LDAP pueden ser el origen de Synology RADIUS Server. 

+ **_VPN Server:_** VPN Server es un paquete complementario que permite que Synology NAS se convierta en un servidor VPN (red privada virtual), permitiendo que los usuarios de DSM accedan a través de Internet a recursos compartidos dentro de la red del área local de los productos de Synology.

+ **_Directory Server:_** Directory Server es un paquete complementario basado en la versión LDAP 3 (RFC2251) que permite que su Synology NAS se convierta en un centro de administración de cuentas de todos los clientes que se conectan y ofrece un servicio de autenticación para ellos.

+ **_Servidor NTP:_** Configuraremos nuestro NAS como servidor NTP para que cualquier equipo o dispositivo en nuestra red tenga la hora sincronizada.

+ **_Mail Plus Server:_** MailPlus Server le permite ejecutar y administrar un servicio de correo de propiedad privada en un Synology NAS. Es compatible con infraestructura high-availability para ofrecer conmutación por error, que permite maximizar el tiempo de actividad de los servicios cuando se producen desastres. Con MailPlus Server, puede hacer lo siguiente:  
	- Supervisar el tráfico de correo general y administrar los servidores de correo emparejados  
	- Administrar la configuración general del sistema, desde los alias a los protocolos, para la entrega de correo (p. ej. SMTP)  
	- Migrar correos electrónicos e importar configuraciones del sistema de otros servidores de correo  
	- Obtener análisis estadísticos y de ubicación de amenazas de correo  
	- Proteger el sistema de correo con antivirus, antispam, SPF, DKIM, DMARC y más mecanismos de seguridad.  
	- Ver y administrar los registros del sistema  
	- Administra las licencias de MailPlus Server  
	- Administrar usuarios/grupos junto con sus métodos de uso e inicio de sesión de correo electrónico  

+ **_Mail Plus:_** Synology MailPlus es un servicio de correo web que ofrece una gran variedad de potentes funciones. Puede enviar y recibir correo electrónico manteniendo todos sus mensajes en un lugar cómodo y seguro. MailPlus le permite generar contenido avanzado mediante herramientas de edición de texto y adhesivos. También incluye etiquetas y exhaustivas funciones de búsqueda que le permiten buscar y gestionar sus mensajes de correo con eficacia. También puede utilizarlo en combinación con MailPlus en iOS y Android.

+ **_Web Server Station:_** El NAS cuenta con una amplia gama de funciones de alojamiento web. Con Web Station puede alojar y publicar su propia página web compatible con un host virtual y configuración adicional HTTP/HTTPS para cada instancia única. También le ofrece flexibilidad para seleccionar el servidor back-end y la configuración de PHP para cada host virtual creado para desarrollar sitios web dinámicos y basados en datos para su uso personal o empresarial.

+ **_Servidor DHCP:_** El NAS puede actuar como un servidor DHCP y asigna direcciones IP dinámicas a clientes DHCP dentro de su red de área local.

+ **_Chat:_** Chat, un servicio de mensajería instantánea en Synology NAS, proporciona a los usuarios una herramienta eficaz para la comunicación y la gestión de la información. Chat ofrece prestaciones entretenidas como hashtags y reacciones, y también muchas otras características como integración personalizada con webhooks entrantes y salientes.

La instalación de las herramientas antes mencionadas no tiene mayo secreto, lo único que necesitas es abrir el `Centro de paquetes` y pulsar el botón instalar para cada una de las herramientas antes mencionadas:

![imagen](/DSM-Virtual/images/InstalacionHerramientas.jpg)