Tras la virtualización de nuestro NAS DS3617xs tendremos que realizar algunos pasos para dejarlo completamente operativo y funcional.

# Crear un Nuevo RAID

Este es el primer paso de post-virtualización. A continuación, os detallo un poco sobre los RAID que permite montar Synology para de esta manera continuar con el proceso de creación de un RAID en nuestro NAS.

## Elegir un Tipo de RAID.

RAID (matriz redundante de discos independientes) es una tecnología de almacenamiento de datos que permite combinar varias unidades en un espacio de almacenamiento único. Existen diferentes tipos de RAID, cada uno con diferentes niveles de rendimiento, capacidad de almacenamiento y fiabilidad.

Este artículo proporciona una breve visión general de los tipos de RAID compatibles con Synology NAS e incluye los requisitos de implementación, además de sus ventajas y desventajas.

## Tipos de RAID compatibles con Synology.

Esta tabla proporciona una breve visión general de diferentes tipos de RAID compatibles con Synology NAS e incluye la capacidad de almacenamiento, el número mínimo de unidades necesarias para cada tipo de RAID y el número de averías de unidades que se pueden tolerar antes de que se produzca una pérdida de datos

<table> 	<tbody><tr> 		<th>Tipo de volumen</th> 		<th>Número de HDD</th> 		<th>Averías de unidades tolerables</th> 		<th>Descripción</th> 		<th>Capacidad del volumen</th> 	</tr> 	<tr> 		<td rowspan="3">SHR</td> 		<td>1</td> 		<td>0</td> 		<td rowspan="3"> 			<ul> 				<li>Optimiza el tamaño del volumen al combinar unidades de distintos tamaños.</li> 				<li>Proporciona redundancia de datos si el volumen está compuesto de dos o más unidades.</li> 				<li>Recomendado para usuarios principiantes.</li> 			</ul> 		</td> 		<td>1 x (tamaño de HDD)</td> 	</tr> 	<tr> 		<td>2-3</td> 		<td>1</td> 		<td rowspan="2">Optimizada por el sistema.</td> 	</tr> 	<tr> 		<td>≧4</td> 		<td>1-2</td> 	</tr> 	<tr> 		<td>Basic</td> 		<td>1</td> 		<td>0</td> 		<td> 			<ul> 				<li>Compuesto de una unidad como unidad independiente.</li> 				<li>No proporciona redundancia de datos.</li> 			</ul> 		</td> 		<td>1 x (tamaño de HDD)</td> 	</tr> 	<tr> 		<td>JBOD</td> 		<td>≧1</td> 		<td>0</td> 		<td> 			<ul> 				<li>Combina una colección de unidades en un espacio de almacenamiento único, con una capacidad igual a la suma de las capacidades de todas las unidades.</li> 				<li>No proporciona redundancia de datos.</li> 			</ul> 		</td> 		<td>Suma de tamaños de todos los HDD</td> 	</tr> 	<tr> 		<td>RAID 0</td> 		<td>≧2</td> 		<td>0</td> 		<td> 			<ul> 				<li>Ofrece "striping", un proceso por el cual los datos se dividen en bloques y se distribuyen por distintas unidades para mejorar el rendimiento.</li> 				<li>No proporciona redundancia de datos.</li> 			</ul> 	    </td> 		<td>Suma de tamaños de todos los HDD</td> 	</tr> 	<tr> 		<td rowspan="3">RAID 1</td> 		<td>2</td> 		<td>1</td> 		<td rowspan="3"> 			<ul> 				<li>Escribe datos idénticos en ambas unidades simultáneamente.</li> 				<li>Proporciona redundancia de datos.</li> 			</ul> 		</td> 		<td rowspan="3">Tamaño del menor HDD</td> 	</tr> 	<tr> 		<td>3</td> 		<td>2</td> 	</tr> 	<tr> 		<td>4</td> 		<td>3</td> 	</tr> 	<tr> 		<td>RAID 5</td> 		<td>≧3</td> 		<td>1</td> 		<td> 			<ul> 				<li>Utiliza striping a nivel de bloque con paridad de datos distribuidos por todas las unidades integrantes, lo que proporciona una redundancia de datos más eficiente que RAID 1.</li> 			</ul> 		</td> 		<td>(N – 1) x (tamaño del HDD más pequeño)</td> 	</tr> 	<tr> 		<td>RAID 6</td> 		<td>≧4</td> 		<td>2</td> 		<td> 			<ul> 				<li>Utiliza la paridad de dos capas de datos para almacenar datos redundantes en un espacio igual al tamaño de dos unidades, proporcionando un mayor grado de redundancia de datos que RAID 5.</li> 			</ul> 		</td> 		<td>(N – 2) x (tamaño del HDD más pequeño)</td> 	</tr> 	<tr> 		<td>RAID 10</td> 		<td>≧4<br>(número par)</td> 		<td>La mitad del n.º total de HDD</td> 		<td> 			<ul> 				<li>Proporciona el rendimiento de RAID 0 y el nivel de protección de datos de RAID 1, mediante la combinación de unidades en grupos de dos, en los que se copian los datos.</li> 			</ul> 		</td> 		<td>(N / 2) x (tamaño del HDD más pequeño)</td> 	</tr> 	<tr> 		<td>RAID F1</td> 		<td>≧3</td> 		<td>1</td> 		<td> 			<ul> 				<li>Utiliza striping a nivel de bloque con paridad de datos distribuidos por todas las unidades integrantes.</li> 				<li>Escribe más información de paridad en una determinada unidad.</li> 				<li>Se recomienda su uso para las matrices completamente Flash.</li> 			</ul> 		</td> 		<td>(N – 1) x (tamaño del HDD más pequeño)</td> 	</tr> </tbody></table>

>Observación:
>    + Los tipos de RAID, excepto "Basic", solo están disponibles en ciertos modelos, dependiendo del número de ranuras de unidades y del número de unidades instaladas.
>    + "N" representa el número total de unidades en el volumen.

### Synology Hybrid RAID (SHR)

Synology Hybrid RAID (SHR) es un sistema de administración RAID automatizado, diseñado para simplificar la administración del almacenamiento y satisfacer las necesidades de nuevos usuarios que no estén familiarizados con los tipos de RAID.

SHR puede combinar unidades diferentes tamaños para crear un volumen de almacenamiento con una capacidad y un rendimiento optimizados, malgastando menos espacio de las unidades y ofreciendo una solución de almacenamiento más flexible. Cuando se incluyen suficientes unidades, SHR permite la redundancia de 1 o 2 discos, lo cual significa que el volumen de SHR puede sufrir hasta una o dos unidades averiadas sin que se produzca ninguna pérdida de datos.

![alt text](https://github.com/rafaeljimenez85/NAS-MiniCorp/blob/master/DSM-Virtual/images/SHR-1-2.png)

### RAID 0

RAID 0 combina dos o más unidades para aumentar el rendimiento y la capacidad, pero no ofrece tolerancia a los fallos. Una avería única en una unidad resultará en la pérdida de todos los datos de la serie. RAID 0 es útil en sistemas no críticos, donde se necesita un equilibrio entre precio/rendimiento elevado.

![alt text](https://github.com/rafaeljimenez85/NAS-MiniCorp/blob/master/DSM-Virtual/images/RAID-0.png)

### RAID 1

RAID 1 es el sistema utilizado más a menudo, con dos unidades. Los datos de las unidades se copian, proporcionando una tolerancia a fallos en caso de avería en las unidades. El rendimiento de lectura se aumenta, mientras que el de escritura es similar al de una unidad. Se puede sufrir un fallo único de unidad sin pérdida de datos. RAID 1 se utiliza a menudo cuando la tolerancia a errores es importante y el espacio y el rendimiento no son requisitos fundamentales.

![alt text](https://github.com/rafaeljimenez85/NAS-MiniCorp/blob/master/DSM-Virtual/images/RAID-1.png)

### RAID 5

RAID 5 proporciona tolerancia a fallos y un mayor rendimiento de lectura. Se necesita un mínimo de tres unidades. RAID 5 puede soportar la pérdida de una unidad. En caso de producirse un fallo de unidad, los datos de la unidad averiada se reconstruyen a partir de la paridad guardada (mediante striping) en las unidades restantes. Como resultado de ello, el rendimiento tanto de lectura como de escritura sufre un gran impacto mientras la matriz RAID 5 se encuentra en un estado degradado. RAID 5 es ideal cuando el espacio y el coste son más importantes que el rendimiento.

![alt text](https://github.com/rafaeljimenez85/NAS-MiniCorp/blob/master/DSM-Virtual/images/RAID-5.png)

### RAID 6

RAID 6 es similar a RAID 5, a diferencia de que proporciona una capa adicional de striping y puede tolerar dos fallos de disco duro. Se necesita un mínimo de cuatro unidades. El rendimiento de RAID 6 es menor que el de RAID 5 debido a esta tolerancia a fallos adicional. RAID 6 resulta atractivo cuando el espacio y el coste son importantes y cuando se necesita poder soportar varios fallos de unidades.

![alt text](https://github.com/rafaeljimenez85/NAS-MiniCorp/blob/master/DSM-Virtual/images/RAID-6.png)

### RAID 10

RAID 10 combina los beneficios de RAID 1 y RAID 0. El rendimiento de lectura y escritura aumentan, pero solo la mitad del espacio total está disponible para almacenar datos. Se necesitan cuatro o más unidades, por lo que el coste es relativamente elevado, pero el rendimiento es muy grande, a la vez que se proporciona tolerancia a los fallos. De hecho, RAID 10 puede hacer frente a varias averías de unidades, siempre y cuando estas no ocurran en el mismo subgrupo. RAID 10 es ideal para aplicaciones con una gran demanda de salida y entrada, como en el caso de servidores de bases de datos.

![alt text](https://github.com/rafaeljimenez85/NAS-MiniCorp/blob/master/DSM-Virtual/images/RAID-10.png)

### RAID F1

RAID F1 aplica el mecanismo de RAID 5, lo que proporciona tolerancia a fallos y un mayor rendimiento de lectura. Sin embargo, con RAID F1 el sistema escribirá más información de paridad en una determinada unidad para acelerar envejecimiento y, por tanto, evitar que todas las unidades lleguen al final de su vida útil al mismo tiempo. Esto puede afectar sutilmente a su rendimiento si se compara con RAID 5. Se necesita un mínimo de tres unidades. RAID F1 puede soportar la pérdida de una unidad. En caso de producirse un fallo de unidad, los datos de la unidad averiada se reconstruyen a partir de la paridad guardada (mediante striping) en las unidades restantes. Como resultado de ello, el rendimiento tanto de lectura como de escritura sufre un gran impacto mientras la matriz RAID F1 se encuentra en un estado degradado. RAID F1 es ideal para las matrices completamente Flash.

![alt text](https://github.com/rafaeljimenez85/NAS-MiniCorp/blob/master/DSM-Virtual/images/RAID-F1.png)

> Observación:
>    + RAID F1 solo está disponible en modelos específicos. Para ver si su modelo es compatible con RAID F1, visite el [sitio web de Synology](https://www.synology.com/) y compruebe las especificaciones de su modelo.

## Configuración del nuevo RAID.

Tras analizar los datos proporcionados anteriormente y dado que se trata de un NAS que no hará las tareas propiamente de un NAS que es almacenar datos, sino, los usaremos para correr aplicación de seguridad he elegido que sea un RAID de tipo Basic, para configurarlo realizaremos los siguientes pasos:

+ Desde la Pagina web de nuestro NAS desplegaremos el menú y seleccionaremos la herramienta `Administrador de Almacenamiento`  
	![alt text](https://github.com/rafaeljimenez85/NAS-MiniCorp/blob/master/DSM-Virtual/images/paso39.jpg)

+ Nos dirigiremos a `RAID Group` y `Crear`  
	![alt text](https://github.com/rafaeljimenez85/NAS-MiniCorp/blob/master/DSM-Virtual/images/paso40.jpg)

+ Seleccionaremos la opción `RAID Group para un único volumen o iSCSI o LUN (Nivel de Bloques)`  
	![alt text](https://github.com/rafaeljimenez85/NAS-MiniCorp/blob/master/DSM-Virtual/images/paso41.jpg)

+ En la descripción pondremos el nombre que deseamos para el RAID en este `Básico`, en el Tipo de RAID elegiremos `Basic`  
	![alt text](https://github.com/rafaeljimenez85/NAS-MiniCorp/blob/master/DSM-Virtual/images/paso42.jpg)

+ Moveremos de la tabla con el nombre del NAS en este caso `NAS-LAB-1` a la parte de la tabla `Raid Group` el disco `SATA de 10GB` que hemos creado a la hora de configurar la máquina virtual   
	![alt text](https://github.com/rafaeljimenez85/NAS-MiniCorp/blob/master/DSM-Virtual/images/paso43.jpg)

+ Aceptaremos la advertencia que indica que se borraran los datos del disco  
	![alt text](https://github.com/rafaeljimenez85/NAS-MiniCorp/blob/master/DSM-Virtual/images/paso44.jpg)

+ Indicamos que `NO` deseamos hacer la comprobación de sectores erróneos.  
	![alt text](https://github.com/rafaeljimenez85/NAS-MiniCorp/blob/master/DSM-Virtual/images/paso45.jpg)

+ Verificaremos que los datos estén correctos y posteriormente pulsaremos en aplicar.  
	![alt text](https://github.com/rafaeljimenez85/NAS-MiniCorp/blob/master/DSM-Virtual/images/paso46.jpg)

+ Una vez creado el RAID nos indicara que debemos crear un Volumen. Este será el siguiente paso de la configuración post-virtualización.  
	![alt text](https://github.com/rafaeljimenez85/NAS-MiniCorp/blob/master/DSM-Virtual/images/paso47.jpg)


# Configuración de un nuevo volumen 

Los volúmenes son espacios de almacenamiento básico en los que el usuario puede crear carpetas compartidas o iSCSI LUN (archivo normal), guardar datos o instalar paquetes. Antes de crear cualquier volumen, asegúrese de que ya ha creado un RAID Group. Para obtener más información sobre RAID Group

## ¿Qué sistema de archivos debo utilizar para crear un volumen?

Los sistemas de archivos ext4 y Btrfs1 son compatibles con DSM 6.0 o posterior. Puede crear un volumen en cualquiera de estos sistemas de archivos conforme a sus necesidades y preferencias. A continuación, le mostramos nuestras sugerencias sobre qué sistema de archivos se debe usar para crear un volumen.

<table id="faqtable" cellpadding="3" style="vertical-align: central;">
	<tbody><tr>
		<th style="width: 15%;"></th>
		<th style="width: 45%;">Btrfs</th>
		<th style="width: 40%;">ext4</th>
	</tr>
	<tr>
		<th style="width: 15%;">Características del sistema de archivos</th>
		<td style="width: 45%;">
			<ul>
				<li>Funciones de protección de datos como, por ejemplo, instantáneas, replicación y recuperaciones a un momento dado</li>
				<li>Protección de la integridad de datos</li>
				<li>Cuotas de usuario para cada carpeta compartida</li>
			</ul>
		</td>
		<td style="width: 40%;">
			<ul>
				<li>Compatible con la mayoría de sistemas operativos LINUX</li>
				<li>Mayor rendimiento y menos requisitos de hardware que Btrfs</li>
			</ul>
		</td>
	</tr>
	<tr>
		<th style="width: 15%;">Objetivos principales del volumen</th>
		<td style="width: 45%;">
			<ul>
				<li>Almacene los datos críticos para la empresa que requieran integridad de datos y protección</li>
				<li>Recomendado para el uso compartido de archivos en general, o para proporcionar iSCSI LUN para la virtualización de servidores</li>
				<li>Recomendado para aplicaciones diarias tanto para usuarios domésticos como profesionales</li>
				<li>Activación de Docker DSM</li>
			</ul>
		</td>
		<td style="width: 40%;">
			<ul>
				<li>Uso de almacenamientos compartidos de red para entornos virtualizados o aplicaciones de base de datos</li>
				<li>Uso de los siguientes paquetes o funciones con requisitos orientados al rendimiento:<ul>
						<li>iSCSI LUN</li>
						<li>Servidor de archivos</li>
						<li>MailPlus Server</li>
					</ul>
				</li>
			</ul>
		</td>
	</tr>
</tbody></table>


## Crear el nuevo volumen

Para crear el nuevo volumen seguiremos los siguientes pasos:

+ Sobre la misma herramienta que hemos trabajado anteriormente `Administrador de Almacenamiento`, iremos al menú Volumen y pulsaremos en `Crear`  
	![alt text](https://github.com/rafaeljimenez85/NAS-MiniCorp/blob/master/DSM-Virtual/images/paso48.jpg)

+ Elegimos el RAID que hemos creado con anterioridad `RAID Group 1`, indicaremos la descripción que deseamos y escogemos el sistema de archivos `Btrfs`  
	![alt text](https://github.com/rafaeljimenez85/NAS-MiniCorp/blob/master/DSM-Virtual/images/paso49.jpg)

+ Ya tendríamos nuestro volumen montado sobre el RAID que habíamos creado en los pasos anteriores.  
	![alt text](https://github.com/rafaeljimenez85/NAS-MiniCorp/blob/master/DSM-Virtual/images/paso50.jpg)

