# 1. Introducción

Con este trabajo busco demostrar que por un precio razonable una Micro PYME de no más de 8 - 10 empleados pueden pasar de no tener un entrono nada seguro a tener una seguridad muy aceptable para la red y los empleados de la misma, Tras analizar diferentes herramientas, opte por usar un NAS **Synology DS3617xs** que tiene las características que se definen al final de la página.

Me decido a utilizar un NAS de Synology porque tras analizar las herramientas que trae su Sistema Operativo (**DSM versión 6.1.6**) me sorprende la gran cantidad de herramientas con las que cuenta tanto para poder realizar la segurización de nuestro entorno empresarial como también las herramientas que para una empresa son esenciales, sin contar que al tratarse de un NAS contaran con almacenamiento y seguridad en sus datos.

Dado que no contaba con un NAS synology DS3617xs de sobra, decidí que la mejor opción para realizar este trabajo esta Virtualizar el Sistema Operativo DSM con VMware Workstation, Esta virtualización la realice con el único objetivo de realizar el laboratorio de pruebas, pero no es viable la virtualización del mismo para un entorno corporativo o empresarial dado que incumple las políticas de Uso de DSM establecidas por Synology.

A lo largo de este trabajo se definirá de manera muy realista un montaje desde 0 de una Micro Pyme (Ficticia) llamada **Tripode Corp** en el que su dominio es tripodecorp.black y se crearan desde usuarios corporativos de LDAP pasando por los certificados del dominio y acabando con acceso VPN a la red de la empresa y la publicación de su página WEB. Las herramientas que se utilizaran son las siguientes:

+ **Herramientas de Seguridad.**
	- _Proxy Server_
	- _DHCP Server_
	- _Radius Server_
	- _Directory Server (LDAP)_
	- _Firewall_
	- _Certificados_
	- _VPN Server_
+ **Herramientas Coorporativas**
	- _Mail Server_
	- _Chat Server_
	- _Web Server_
+ **Otras Herramientas**
	- _Servidor NTP_

Sin más dilación empecemos con el trabajo de campo.


## 1.2. Tabla de especificaciones de Synology DS3617xs


<div class="table_wrap">
		<table class="spec_table">
							
				<tbody><tr class="th1">
					<td colspan="2">
						Especificaciones de hardware
						</td>
				</tr>
																	<tr class="th2">
							<td colspan="2">CPU</td>
						</tr>
																				<tr>
		<td>modelo de CPU</td>

					<td>
			Intel Xeon D-1527
		</td>
			
	</tr>

														<tr>
		<td>arquitectura de CPU</td>

					<td>
			64-bit
		</td>
			
	</tr>

														<tr>
		<td>Frecuencia de CPU</td>

			<td>
		Núcleo cuádruple 2,2&nbsp; (base) / 2,7&nbsp; (turbo) GHz
		</td>
	
	</tr>

														<tr>
		<td>Motor de cifrado de hardware (AES-NI)</td>

			<td class="vmiddle">
			<i class="fa fa-check"></i>
		</td>
	
	</tr>

																							<tr class="th2">
							<td colspan="2">Memoria</td>
						</tr>
																				<tr>
		<td>Memoria del sistema</td>

			<td>
		16 GB DDR4 ECC SO-DIMM
		</td>
	
	</tr>

														<tr>
		<td>Módulo de memoria preinstalado</td>

					<td>
			16GB (2 x 8GB)
		</td>
			
	</tr>

														<tr>
		<td>Ranuras de memoria totales</td>

			<td>
		4
		</td>
	
	</tr>

														<tr>
		<td>Memoria ampliable hasta</td>

					<td>
			48 GB (16 GB + 16 GB x 2)
		</td>
			
	</tr>

																							<tr class="th2">
							<td colspan="2">Almacenamiento</td>
						</tr>
																				<tr>
		<td>Receptáculo(s) de unidad</td>

			<td>
		12
		</td>
	
	</tr>

														<tr>
		<td>Receptáculos de unidad máx. con la unidad de expansión</td>

			<td>
		36
		</td>
	
	</tr>

														<tr>
		<td>Tipo de unidad compatible* <a href="/es-es/compatibility"> (ver todas las unidades de disco duro compatibles)</a></td>

									<td class="list">
				<ul>
									<li>3.5" SATA HDD</li>
									<li>2.5" SATA HDD </li>
									<li>2.5" SATA SSD</li>
								</ul>
			</td>
						
	</tr>

														<tr>
		<td>Capacidad máxima bruta interna</td>

			<td>
		144 TB (unidad de disco duro de 12 TB x 12) (la capacidad puede variar según el tipo de RAID)
		</td>
	
	</tr>

														<tr>
		<td>Capacidad máxima bruta con unidades de expansión</td>

			<td>
		432 TB 
(144 TB + 12 TB drive x 24) (la capacidad puede variar según el tipo de RAID)
		</td>
	
	</tr>

														<tr>
		<td>Tamaño máximo de volumen individual</td>

									<td class="list">
				<ul>
									<li>200 TB (se necesitan 32 GB de RAM, solo para grupos RAID 5 o RAID 6)</li>
									<li>108 TB</li>
								</ul>
			</td>
						
	</tr>

														<tr>
		<td>Unidad de intercambio en caliente</td>

			<td class="vmiddle">
			<i class="fa fa-check"></i>
		</td>
	
	</tr>

																							<tr class="th2">
							<td colspan="2">Puertos externos</td>
						</tr>
																				<tr>
		<td>Puerto RJ-45 1GbE LAN</td>

					<td>
			4 (Con soporte de Conmutación por error / Link Aggregation)
		</td>
			
	</tr>

														<tr>
		<td>Puerto USB 3.0</td>

			<td>
		2
		</td>
	
	</tr>

														<tr>
		<td>Puerto de expansión</td>

			<td>
		2
		</td>
	
	</tr>

																							<tr class="th2">
							<td colspan="2">PCIe</td>
						</tr>
																				<tr>
		<td>Expansión PCIe</td>

										<td>
					1 x Gen3 x8 slot (x8 link)
				</td>
						
	</tr>

														<tr>
		<td>Compatibilidad con tarjeta de ampliación</td>

										<td>
					Tarjeta de interfaz de red PCIe<a href="/es-es/compatibility?search_by=category&amp;category=network_interface_cards&amp;p=1"> (Obtenga más información) </a>
				</td>
						
	</tr>

																							<tr class="th2">
							<td colspan="2">Sistema de archivos</td>
						</tr>
																				<tr>
		<td>Unidades internas</td>

									<td class="list">
				<ul>
									<li>Btrfs</li>
									<li>EXT4</li>
								</ul>
			</td>
						
	</tr>

														<tr>
		<td>Unidades externas</td>

									<td class="list">
				<ul>
									<li>Btrfs</li>
									<li>EXT4</li>
									<li>EXT3</li>
									<li>FAT</li>
									<li>NTFS</li>
									<li>HFS+</li>
									<li>exFAT*</li>
								</ul>
			</td>
						
	</tr>

														<tr>
		<td>Notas</td>

										<td>
					exFAT Access se adquiere por separado en el Centro de paquetes.
				</td>
						
	</tr>

																							<tr class="th2">
							<td colspan="2">Apariencia</td>
						</tr>
																				<tr>
		<td>Tamaño (Altura x Anchura x Profundidad)</td>

			<td>
		270 mm x 300 mm x 340 mm <br>
		</td>
	
	</tr>

														<tr>
		<td>Peso</td>

			<td>
		9.8 kg <br>
		</td>
	
	</tr>

																							<tr class="th2">
							<td colspan="2">Otros</td>
						</tr>
																				<tr>
		<td>Ventilador del sistema</td>

			<td>
		120 mm x 120 mm x 2 pcs
		</td>
	
	</tr>

														<tr>
		<td>Modo de velocidad de ventilador</td>

									<td class="list">
				<ul>
									<li>Modo Velocidad completa</li>
									<li>Modo fresco</li>
									<li>Modo silencioso</li>
								</ul>
			</td>
						
	</tr>

														<tr>
		<td>Fácil sustitución de ventilador del sistema</td>

			<td class="vmiddle">
			<i class="fa fa-check"></i>
		</td>
	
	</tr>

														<tr>
		<td>Recuperación de energía</td>

			<td class="vmiddle">
			<i class="fa fa-check"></i>
		</td>
	
	</tr>

														<tr>
		<td>Nivel de ruido*</td>

			<td>
		22.3 dB(A) <br>
		</td>
	
	</tr>

														<tr>
		<td>Encendido y apagado programado</td>

			<td class="vmiddle">
			<i class="fa fa-check"></i>
		</td>
	
	</tr>

														<tr>
		<td>Despertar con LAN/WAN</td>

			<td class="vmiddle">
			<i class="fa fa-check"></i>
		</td>
	
	</tr>

														<tr>
		<td>Fuente / Adaptador de alimentación</td>

			<td>
		500W <br>
		</td>
	
	</tr>

														<tr>
		<td>Voltaje de alimentación de entrada CA</td>

			<td>
		De 100V a 240V CA
		</td>
	
	</tr>

														<tr>
		<td>Frecuencia de alimentación</td>

			<td>
		50/60 Hz, Monofásico
		</td>
	
	</tr>

														<tr>
		<td>Consumo de energía*</td>

			<td>
		108.2 W (Acceso) <br> 53.4 W (Hibernación de unidad de disco duro) <br>
		</td>
	
	</tr>

														<tr>
		<td>British thermal unit</td>

			<td>
		369.4 BTU/hr (Acceso) <br> 182.3 BTU/hr (Hibernación de unidad de disco duro) <br>
		</td>
	
	</tr>

																							<tr class="th2">
							<td colspan="2">Temperatura ambiente</td>
						</tr>
																				<tr>
		<td>Temperatura operativa</td>

			<td>
		De 5°C a 40°C (de 40°F a 104°F)
		</td>
	
	</tr>

														<tr>
		<td>Temperatura de almacenamiento</td>

			<td>
		De -20°C a 60°C (de -5°F a 140°F)
		</td>
	
	</tr>

														<tr>
		<td>Humedad relativa</td>

			<td>
		De 5% a 95% HR
		</td>
	
	</tr>

																																	<tr class="th2">
		<td>Certificación</td>

									<td class="list">
				<ul>
									<li>EAC</li>
									<li>VCCI</li>
									<li>RCM</li>
									<li>KC</li>
									<li>FCC</li>
									<li>CE</li>
									<li>BSMI</li>
								</ul>
			</td>
						
	</tr>

																																														<tr class="th2">
		<td>Garantía</td>

					<td>
			5  años
		</td>
			
	</tr>

																																														<tr class="th2">
		<td>Notas</td>

									<td class="list">
				<ul>
									<li>Consumo de energía medido cuando la unidad (o unidades) de disco duro Western Digital 1TB WD10EFRX se ha(n) cargado completamente.</li>
									<li>Entorno de comprobación del nivel de ruido: totalmente cargado con disco(s) duro(s) Seagate 2TB ST2000VN000 en funcionamiento; dos micrófonos G.R.A.S. Type 40AE, configurado cada uno a 1 metro de distancia de la parte frontal y posterior de Synology NAS; ruido de fondo: 16,49-17,51 dB(A); Temperatura: 24,25-25,75˚C; Humedad: 58,2-61,8%.</li>
								</ul>
			</td>
						
	</tr>

																												
							
				<tr class="th1">
					<td colspan="2">
						Especificaciones de DSM
						<a href="#" id="dsm_spec_dialog">( Ver más )</a>
						</td>
				</tr>
																	<tr class="th2">
							<td colspan="2">Gestión de almacenamiento</td>
						</tr>
																				<tr>
		<td>Número máx. de volumen interno</td>

			<td>
		1024
		</td>
	
	</tr>

														<tr>
		<td>Número máx. de iSCSI Target</td>

			<td>
		64
		</td>
	
	</tr>

														<tr>
		<td>Número máx. de iSCSI LUN</td>

			<td>
		512
		</td>
	
	</tr>

														<tr>
		<td>Clon/Instantánea de iSCSI LUN, Windows ODX (Las transferencias de datos descargados)</td>

			<td class="vmiddle">
			<i class="fa fa-check"></i>
		</td>
	
	</tr>

														<tr>
		<td>SSD de caché de lectura/escritura <a href="http://global.download.synology.com/download/Document/WhitePaper/Synology_SSD_Cache_White_Paper.pdf"> (White Paper) </a></td>

			<td class="vmiddle">
			<i class="fa fa-check"></i>
		</td>
	
	</tr>

														<tr>
		<td>SSD TRIM</td>

			<td class="vmiddle">
			<i class="fa fa-check"></i>
		</td>
	
	</tr>

														<tr>
		<td>RAID Group</td>

			<td class="vmiddle">
			<i class="fa fa-check"></i>
		</td>
	
	</tr>

														<tr>
		<td>Tipo de RAID compatible</td>

									<td class="list">
				<ul>
									<li>RAID F1</li>
									<li>Basic</li>
									<li>JBOD</li>
									<li>RAID 0</li>
									<li>RAID 1</li>
									<li>RAID 5</li>
									<li>RAID 6</li>
									<li>RAID 10</li>
								</ul>
			</td>
						
	</tr>

														<tr>
		<td>Migración de RAID</td>

									<td class="list">
				<ul>
									<li>Basic to RAID 1</li>
									<li>Basic to RAID 5</li>
									<li>RAID 1 to RAID 5</li>
									<li>RAID 5 to RAID 6</li>
								</ul>
			</td>
						
	</tr>

														<tr>
		<td>Expansión de volumen con unidades de disco duro más grandes</td>

									<td class="list">
				<ul>
									<li>RAID F1</li>
									<li>RAID 1</li>
									<li>RAID 5</li>
									<li>RAID 6</li>
								</ul>
			</td>
						
	</tr>

														<tr>
		<td>Expansión de volumen añadiendo una unidad de disco duro</td>

									<td class="list">
				<ul>
									<li>RAID F1</li>
									<li>RAID 5</li>
									<li>RAID 6</li>
									<li>JBOD</li>
								</ul>
			</td>
						
	</tr>

														<tr>
		<td>Tipo de RAID compatible con Global Hot Spare</td>

									<td class="list">
				<ul>
									<li>RAID F1</li>
									<li>RAID 1</li>
									<li>RAID 5</li>
									<li>RAID 6</li>
									<li>RAID 10</li>
								</ul>
			</td>
						
	</tr>

																							<tr class="th2">
							<td colspan="2">Capacidad de intercambio de archivos</td>
						</tr>
																				<tr>
		<td>Núm. máximo de cuentas de usuario local</td>

			<td>
		16000
		</td>
	
	</tr>

														<tr>
		<td>Núm. máximo de grupos locales</td>

			<td>
		512
		</td>
	
	</tr>

														<tr>
		<td>Número máx. de carpetas compartidas</td>

			<td>
		512
		</td>
	
	</tr>

														<tr>
		<td>Número máx. de tareas de sincronización de carpetas compartidas</td>

			<td>
		16
		</td>
	
	</tr>

														<tr>
		<td>Número máx. de conexiones CIFS/AFP/FTP simultáneas</td>

			<td>
		2048
		</td>
	
	</tr>

														<tr>
		<td>Integración con Lista de control de acceso de Windows (ACL)</td>

			<td class="vmiddle">
			<i class="fa fa-check"></i>
		</td>
	
	</tr>

														<tr>
		<td>Autenticación Kerberos de NFS</td>

			<td class="vmiddle">
			<i class="fa fa-check"></i>
		</td>
	
	</tr>

																																	<tr class="th2">
		<td>Administrador de High Availability</td>

			<td class="vmiddle">
			<i class="fa fa-check"></i>
		</td>
	
	</tr>

																																														<tr class="th2">
		<td>Centro de registros</td>

			<td class="vmiddle">
			<i class="fa fa-check"></i>
		</td>
	
	</tr>

																																														<tr class="th3">
		<td>Eventos Syslog por segundo</td>

			<td>
		3000
		</td>
	
	</tr>

																																				<tr class="th2">
							<td colspan="2">Virtualización</td>
						</tr>
																				<tr>
		<td>VMware vSphere 6 with VAAI</td>

			<td class="vmiddle">
			<i class="fa fa-check"></i>
		</td>
	
	</tr>

														<tr>
		<td>Windows Server 2012</td>

			<td class="vmiddle">
			<i class="fa fa-check"></i>
		</td>
	
	</tr>

														<tr>
		<td>Windows Server 2012 R2</td>

			<td class="vmiddle">
			<i class="fa fa-check"></i>
		</td>
	
	</tr>

														<tr>
		<td>Citrix Ready</td>

			<td class="vmiddle">
			<i class="fa fa-check"></i>
		</td>
	
	</tr>

														<tr>
		<td>OpenStack</td>

			<td class="vmiddle">
			<i class="fa fa-check"></i>
		</td>
	
	</tr>

															
							
				<tr class="th1">
					<td colspan="2">
						Paquetes complementarios <a href="/es-es/dsm/packages">(obtenga más información sobre la lista completa de paquetes complementarios)</a>
						</td>
				</tr>
																											<tr class="th2">
		<td>Antivirus by McAfee (Trial)</td>

			<td class="vmiddle">
			<i class="fa fa-check"></i>
		</td>
	
	</tr>

																																														<tr class="th2">
		<td>Central Management System</td>

			<td class="vmiddle">
			<i class="fa fa-check"></i>
		</td>
	
	</tr>

																																														<tr class="th2">
		<td>Chat</td>

			<td class="vmiddle">
			<i class="fa fa-check"></i>
		</td>
	
	</tr>

																																														<tr class="th3">
		<td>Número máximo de usuarios</td>

			<td>
		4000
		</td>
	
	</tr>

																																														<tr class="th3">
		<td>Notas</td>

									<td class="list">
				<ul>
									<li>El número de conexiones HTTP simultáneas para Chat se ha configurado al número máximo.</li>
									<li>El uso de la CPU y la RAM fue inferior al 80% al probar el número de clientes recomendado.</li>
									<li>Para los modelos probados con memoria ampliable, se instaló la máxima cantidad RAM.</li>
								</ul>
			</td>
						
	</tr>

																																														<tr class="th2">
		<td>Cloud Station Server</td>

			<td class="vmiddle">
			<i class="fa fa-check"></i>
		</td>
	
	</tr>

																																														<tr class="th3">
		<td>Número máximo de dispositivos conectados simultáneamente</td>

			<td>
		2000
		</td>
	
	</tr>

																																														<tr class="th3">
		<td>Número máximo de dispositivos conectados simultáneamente (con ampliación de RAM)</td>

			<td>
		10000
		</td>
	
	</tr>

																																														<tr class="th3">
		<td>Número máximo de archivos sincronizados (btrfs)</td>

			<td>
		1,000,000 / tiempo de reacción de un archivo: 542 ms
		</td>
	
	</tr>

																																														<tr class="th3">
		<td>Número máximo de archivos sincronizados (ext4)</td>

			<td>
		1,000,000 / tiempo de reacción de un archivo: 210 ms
		</td>
	
	</tr>

																																														<tr class="th3">
		<td>Notas</td>

									<td class="list">
				<ul>
									<li>El número máximo de dispositivos conectados simultáneamente hace referencia al máximo de dispositivos que pueden permanecer conectados al mismo tiempo. Para obtener más información sobre la capacidad de procesamiento de archivos, consulte el tiempo de reacción para el procesamiento de archivos.</li>
									<li>El tiempo de reacción de archivo se refiere al tiempo de preparación necesario para que otros dispositivos empiecen a descargar un archivo de 10KByte de un Synology NAS después de añadir dicho archivo al NAS. No se utilizó la ampliación de la RAM durante la prueba.</li>
									<li>No se utilizaron carpetas compartidas no cifradas durante la prueba mencionada anteriormente.</li>
								</ul>
			</td>
						
	</tr>

																																														<tr class="th2">
		<td>Document Viewer</td>

			<td class="vmiddle">
			<i class="fa fa-check"></i>
		</td>
	
	</tr>

																																														<tr class="th2">
		<td>Download Station</td>

			<td class="vmiddle">
			<i class="fa fa-check"></i>
		</td>
	
	</tr>

																																														<tr class="th3">
		<td>Número máx. de tareas de descarga simultáneas</td>

			<td>
		80
		</td>
	
	</tr>

																																														<tr class="th2">
		<td>Drive</td>

			<td class="vmiddle">
			<i class="fa fa-check"></i>
		</td>
	
	</tr>

																																														<tr class="th3">
		<td>Número máximo de archivos sincronizados</td>

			<td>
		3,000,000 / tiempo de reacción de un archivo: 244 ms
		</td>
	
	</tr>

																																														<tr class="th3">
		<td>Número máximo de conexiones simultáneas para clientes de PC</td>

			<td>
		1,800
		</td>
	
	</tr>

																																														<tr class="th3">
		<td>Notas</td>

									<td class="list">
				<ul>
									<li>Se ha utilizado el sistema de archivos Btrfs con el propósito de realizar pruebas.</li>
									<li>El número máximo de conexiones&nbsp;simultáneas se refiere al número máximo de conexiones que pueden mantenerse al alcanzar el número máximo de archivos sincronizados.</li>
									<li>El tiempo de reacción de archivo se refiere al tiempo de preparación necesario para que otros dispositivos empiecen a descargar un archivo de 10KByte de un Synology NAS después de añadir dicho archivo al NAS.</li>
									<li>Para los modelos probados con memoria ampliable, se instaló la máxima cantidad RAM.</li>
									<li>No se utilizaron carpetas compartidas no cifradas durante la prueba mencionada anteriormente.</li>
								</ul>
			</td>
						
	</tr>

																																														<tr class="th2">
		<td>Acceso exFAT (opcional)</td>

			<td class="vmiddle">
			<i class="fa fa-check"></i>
		</td>
	
	</tr>

																																														<tr class="th2">
		<td>MailPlus / MailPlus Server</td>

			<td class="vmiddle">
			<i class="fa fa-check"></i>
		</td>
	
	</tr>

																																														<tr class="th3">
		<td>Cuentas de correo electrónico gratuitas</td>

			<td>
		5 <a href="/es-es/products/MailPlus_License">  (Licencias necesarias para cuentas adicionales) </a>
		</td>
	
	</tr>

																																														<tr class="th3">
		<td>Número de clientes de MailPlus recomendado</td>

			<td>
		Hasta 400 (Btrfs) / 400 (EXT4)
		</td>
	
	</tr>

																																														<tr class="th3">
		<td>Rendimiento máximo del servidor</td>

			<td>
		3,283,000 (Btrfs) / 1,992,000 (EXT4)  correos electrónicos por día, aprox. 100.2 GB (Btrfs) / 60.8 GB (EXT4)
		</td>
	
	</tr>

																																														<tr class="th3">
		<td>Notas</td>

									<td class="list">
				<ul>
									<li>Tanto el uso de la CPU y como de la RAM se mantuvieron por debajo del 80 % durante las pruebas con los números de clientes recomendados.</li>
									<li>Para los modelos probados con memoria ampliable, se instaló la máxima cantidad RAM.</li>
									<li>En los modelos NAS de 5 habitáculos (o más) con 2 unidades SSD instaladas se habilitó la caché de SSD de lectura-escritura.</li>
									<li>El rendimiento del sistema de correo se reduce ligeramente en el modo high-availability debido a la sincronización de datos entre los dos servidores.</li>
									<li>Funciones que se habilitaron en todas las pruebas anteriores: protección contra correo no deseado, antivirus, DNSBL, lista gris, búsqueda de contenido, búsqueda de contenido (solo en inglés).</li>
								</ul>
			</td>
						
	</tr>

																																														<tr class="th2">
		<td>Media Server</td>

			<td class="vmiddle">
			<i class="fa fa-check"></i>
		</td>
	
	</tr>

																																														<tr class="th3">
		<td>DLNA Compliance</td>

			<td class="vmiddle">
			<i class="fa fa-check"></i>
		</td>
	
	</tr>

																																														<tr class="th2">
		<td>Moments</td>

			<td class="vmiddle">
			<i class="fa fa-check"></i>
		</td>
	
	</tr>

																																														<tr class="th3">
		<td>Reconocimiento facial</td>

					<td>
			<i class="fa fa-check"></i>
		</td>
			
	</tr>

																																														<tr class="th3">
		<td>Reconocimiento de sujeto</td>

					<td>
			<i class="fa fa-check"></i>
		</td>
			
	</tr>

																																														<tr class="th2">
		<td>Office</td>

			<td class="vmiddle">
			<i class="fa fa-check"></i>
		</td>
	
	</tr>

																																														<tr class="th3">
		<td>Número máximo de usuarios</td>

			<td>
		2800
		</td>
	
	</tr>

																																														<tr class="th3">
		<td>Notas</td>

									<td class="list">
				<ul>
									<li>Se han abierto varios archivos para realizar pruebas y cada uno de ellos fue editado por 30 usuarios simultáneamente.</li>
									<li>El uso de la CPU y la RAM fue inferior al 80% al probar el número de clientes recomendado.</li>
									<li>Para los modelos probados con memoria ampliable, se instaló la máxima cantidad RAM.</li>
									<li>El rendimiento del cliente puede afectar al número máximo de usuarios de edición simultánea. Los PC de cliente utilizados para las pruebas: Intel Core i3-3220 / 8 GB RAM</li>
								</ul>
			</td>
						
	</tr>

																																														<tr class="th2">
		<td>PetaSpace</td>

			<td class="vmiddle">
			<i class="fa fa-check"></i>
		</td>
	
	</tr>

																																														<tr class="th2">
		<td>Snapshot Replication</td>

			<td class="vmiddle">
			<i class="fa fa-check"></i>
		</td>
	
	</tr>

																																														<tr class="th3">
		<td>Número máximo de instantáneas por carpeta compartida</td>

					<td>
			1024
		</td>
			
	</tr>

																																														<tr class="th3">
		<td>Número máximo de instantáneas de todas las carpetas compartidas</td>

					<td>
			65536
		</td>
			
	</tr>

																																														<tr class="th2">
		<td>Surveillance Station</td>

			<td class="vmiddle">
			<i class="fa fa-check"></i>
		</td>
	
	</tr>

																																														<tr class="th3">
		<td>Número máx. de cámaras IP (licencias necesarias)</td>

			<td>
		75 (incluyendo 2 licencia gratuita) <a href="/es-es/compatibility/camera"> (ver todas las cámaras IP compatibles)</a>
		</td>
	
	</tr>

																																														<tr class="th3">
		<td>FPS total (H.264)*</td>

			<td>
		2250 FPS @ 720p (1280x720) <br> 1050 FPS @ 1080p (1920×1080) <br> 600 FPS @ 3M (2048x1536) <br> 500 FPS @ 5M (2591x1944) <br>
500 FPS @ 4K (3840x2160)
		</td>
	
	</tr>

																																														<tr class="th3">
		<td>FPS total (H.265)*</td>

			<td>
		2250 FPS @ 720p (1280x720) <br> 2250 FPS @ 1080p (1920×1080) <br> 2250 FPS @ 3M (2048x1536) <br> 1875 FPS @ 5M (2591x1944) <br>
975 FPS @ 4K (3840x2160)
		</td>
	
	</tr>

																																														<tr class="th3">
		<td>FPS total (MJPEG)*</td>

			<td>
		900 FPS @ 720p (1280x720) <br> 450 FPS @ 1080p (1920×1080) <br> 280 FPS @ 3M (2048x1536) <br> 200 FPS @ 5M (2591x1944)
		</td>
	
	</tr>

																																														<tr class="th3">
		<td>Notas</td>

					<td>
			El rendimiento de Surveillance Station se ha comprobado con la cámara de red Axis, utilizando la grabación continua con visualización en directo de Surveillance Station y la detección de movimiento mediante cámara; la grabación y la visualización de vídeo en directo al mismo tiempo comparten la misma transmisión de vídeo de la cámara.
		</td>
			
	</tr>

																																														<tr class="th2">
		<td>Video Station</td>

			<td class="vmiddle">
			<i class="fa fa-check"></i>
		</td>
	
	</tr>

																																														<tr class="th3">
		<td>Transcodificación de vídeo</td>

					<td>
			Grupo 1 - Tipo 1 <a href="/es-es/knowledgebase/faq/577"> (Ver más)</a>
		</td>
			
	</tr>

																																														<tr class="th3">
		<td>Número máximo de canales de transcodificación</td>

										<td>
					1 canal, 30 FPS @ 1080p(1920×1080), H.264 (AVC)/MPEG-4 Part 2 (XVID, DIVX5)/MPEG-2/VC-1
				</td>
						
	</tr>

																																														<tr class="th2">
		<td>Virtual Machine Manager</td>

			<td class="vmiddle">
			<i class="fa fa-check"></i>
		</td>
	
	</tr>

																																														<tr class="th3">
		<td>Número máximo de Virtual DSM (licencias necesarias)</td>

			<td>
		8 (incluida 1 licencia gratis) <a href="/es-es/products/VDSM_License_Pack"> (Más información)</a>
		</td>
	
	</tr>

																																														<tr class="th3">
		<td>Notas</td>

					<td>
			Virtual Machine Manager no es compatible con hosts en los que se ejecuta Synology High Availability.
		</td>
			
	</tr>

																																														<tr class="th2">
		<td>VPN Server</td>

			<td class="vmiddle">
			<i class="fa fa-check"></i>
		</td>
	
	</tr>

																																														<tr class="th3">
		<td>Conexiones máximas</td>

			<td>
		30
		</td>
	
	</tr>

																												
							
				<tr class="th1">
					<td colspan="2">
						Entorno y paquete
						</td>
				</tr>
																											<tr class="th2">
		<td>Entorno</td>

					<td>
			Conforme con RoHS
		</td>
			
	</tr>

																																														<tr class="th2">
		<td>Contenido del paquete</td>

									<td class="list">
				<ul>
									<li>Unidad principal X 1</li>
									<li>Paquete de accesorios X 1</li>
									<li>Cable de alimentación CA X 1</li>
									<li>Cable RJ-45 LAN X 2</li>
									<li>Guía de instalación rápida X 1</li>
								</ul>
			</td>
						
	</tr>

																																														<tr class="th2">
		<td>Accesorios opcionales</td>

									<td class="list">
				<ul>
									<li>Surveillance Device License Pack</li>
									<li>RAMEC2133DDR4SO-16G DDR4 ECC SO-DIMM</li>
									<li>Unidad de expansión: <a href="/es-es/products/DX1215">DX1215</a> X 2</li>
									<li>VS360HD</li>
									<li>VS960HD</li>
									<li>Synology Adaptador de Ethernet E10G15-F1</li>
									<li>Synology Adaptador de Ethernet E10G17-F2</li>
								</ul>
			</td>
						
	</tr>

																												
							
				<tr class="th1 dsm_spec">
					<td colspan="2">
						General
						</td>
				</tr>
																											<tr class="th2">
		<td>Protocolos de red</td>

									<td class="list">
				<ul>
									<li>CIFS</li>
									<li>AFP</li>
									<li>NFS</li>
									<li>FTP</li>
									<li>WebDAV</li>
									<li>CalDAV</li>
									<li>iSCSI</li>
									<li>Telnet</li>
									<li>SSH</li>
									<li>SNMP</li>
									<li>VPN (PPTP, OpenVPN)</li>
								</ul>
			</td>
						
	</tr>

																																														<tr class="th2">
		<td>Integración de dominio Windows AD</td>

										<td>
					Inicio de sesión de usuarios de dominio a través de Samba/AFP/FTP/File Station
				</td>
						
	</tr>

																																														<tr class="th2">
		<td>Administración de HDD</td>

									<td class="list">
				<ul>
									<li>Hibernación del disco duro</li>
									<li>S.M.A.R.T.</li>
									<li>Asignación de sector defectuoso dinámico</li>
								</ul>
			</td>
						
	</tr>

																																														<tr class="th2">
		<td>Seguridad</td>

									<td class="list">
				<ul>
									<li>FTP a través de SSL/TLS</li>
									<li>Bloqueo automático de IP</li>
									<li>Cortafuegos</li>
									<li>Copia de seguridad de red cifrada a través de Rsync</li>
									<li>Conexión HTTPS</li>
								</ul>
			</td>
						
	</tr>

																																														<tr class="th2">
		<td>Utilidades</td>

										<td>
					Synology Assistant
				</td>
						
	</tr>

																																														<tr class="th2">
		<td>Clientes compatibles</td>

									<td class="list">
				<ul>
									<li>Windows 7 y 10</li>
									<li>Mac OS X 10.11 onward</li>
								</ul>
			</td>
						
	</tr>

																																														<tr class="th2">
		<td>Exploradores compatibles</td>

									<td class="list">
				<ul>
									<li>Chrome</li>
									<li>Firefox</li>
									<li>Internet Explorer 10 o posterior</li>
									<li>Safari 10 o posterior</li>
									<li>Safari (iOS 10 o posterior)</li>
									<li>Chrome (Android 6.0 o posterior)</li>
								</ul>
			</td>
						
	</tr>

																																														<tr class="th2">
		<td>Idioma compatible</td>

										<td>
					English, Deutsch, Français, Italiano, Español, Dansk, Norsk, Svenska, Nederlands, Русский, Polski, Magyar, Português do Brasil, Português Europeu, Türkçe, Český, 日本語, 한국어, 繁體中文, 简体中文
				</td>
						
	</tr>

																												
					</tbody></table>
	</div> 