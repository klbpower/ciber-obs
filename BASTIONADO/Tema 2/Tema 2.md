Introducción
1. Medidas de seguridad activa
	Proteger la BIOS con contraseña
	Proteger Gestor de arranque con contraseña
	Gestión de usuarios y política de contraseñas
	Autentificación de múltiples factores
	Gestión de permisos
	Cuotas de disco

2. Mecanismos de arranque: BIOS y UEFI
	Arrancar el disco con BIOS o UEFI
	
3. Manejo de particiones
![[particones 1.png]]
La BIOS no firma digitalmente el código firmware. UEFI usa criptografía para verificar el firmware y el código de arranque (SecureBoot).

Cuidado!!
![[Clasificacion_de_bootkits.png]]

![[Vias_de_ataque_UEFI.png]]
Los riesgos son podrían **permitir la ejecución** de software malicioso durante el proceso de arranque

Hay que proteger la BIOS/UEFI
![[proteger-BIOS.png]]
Arranque seguro
 estándar de seguridad destinado a garantizar que los equipos arranquen usando solamente software que sea de **confianza** para el fabricante del equipo
![[bios-uefi.jpg]]

![[interfaz-BIOS.png]]
![[interfaz-UEFI.png]]
![[CCN-CERT_IA-08-15-BIOS (1).pdf]]

![[bios-04.png]]
En caso de que **olvidemos la contraseña de acceso a la BIOS**, se puede resetear la misma mediante la desconexión de la batería CMOS (pila encontrada habitualmente en la placa base). Por esto, es recomendable también establecer una seguridad física del equipo que se esté utilizando.
### **UEFI + Secure Boot**
![[mitigacin-01.png]]

**El arranque de confianza continúa allí donde termina el arranque seguro** **(secure boot)****. El cargador de arranque comprueba la firma digital del kernel de Windows antes de cargarlo.

![[diagrama-bookit (1).png]]
#### 5.3 Grub2. Conseguir acceso root
Para poder acceder ficheros críticos del sistema, tales como **/etc/shadow o** **/etc/passwd**, primero debemos de saber en qué partición se encuentran y como nombrarlas. Podemos saber esto ejecutando **ls o ls -l**
fdisk -l
![[partition.png]]
Luego podemos acceder al fichero deseado con el comando **cat**, de la siguiente forma:
![[cat.png]]
![[edit-grub.png]]
Pulsamos **Ctrl+x** o **F10** y nos dará acceso a una Shell con permisos de administración. Ya en ella es facil ejecutar el comando passwd para modificar la contraseña de cualquier usuario del sistema, incluido el root.
![[access-root.png]]

#### Protección del GRUB
**proteger el gestor de arranque** es que sería posible iniciar sesión por ejemplo editando la configuración de arranque y añadiendo algunas líneas que nos permitan iniciar una terminal:
![[grub.png]]
### Proteger el grub con contraseña

**administrador@orion:~$** cp /boot/grub/grub.cfg ~/grub.cfg.old  
**administrador@orion:~$** cp /etc/grub.d/00_header ~/00_header.old  
**administrador@orion:~$** cp /etc/grub.d/10_linux ~/10_linux.old  
**administrador@orion:~$** cp /etc/grub.d/30_os-prober ~/30_os-prober.old

se edita el fichero **00_header**.
**administrador@orion:~$**sudo nano /etc/grub.d/00_header

Añadiendo las siguientes líneas al final, donde el usuario sería **usuario_grub** y su password **P@ssw0rd**.
cat << EOF  
set superusers="usuario_grub"  
password usuario_grub P@ssw0rd  
EOF 

Estas **contraseñas** se encuentran en **texto plano** y obviamente se deben proteger. Para esto, hay que generar un hash de una contraseña dada y, así, evitar que se puedan obtener fácilmente. El comando es el siguiente:

**administrador@orion:~$** grub-mkpasswd-pbkdf2
![[grub-mkpasswd.png]]
Después de **introducir la contraseña dos veces** se obtiene el **hash** de la contraseña para el usuario "**usuario_grub**", por lo que la tenemos que copiar para utilizarla posteriormente.

grub.pbkdf2.sha512.10000.E3D23C6892BA3602761C7ACBDDEC1AF704659D4DF13D6DD2C67A03692F8492B778  
53D2CE6713488EA1BF90ED43E7C549A76908A72A21D804CBE55475F137E462.E5906C938C97737FEB0FC3140B565013D6D135020865AF549CB3CEDF15C8  
FC842F4C442DDC3ECBA4EA76365ED94C422561107717B538DB10DC93FFAC7E8A1116
la contraseña que teníamos en texto plano en el archivo **/etc/grub.d/00_header**
cat << EOF  
set superusers="usuario_grub"  
**_password_pbkdf2_** usuario_grub _**<salida que hemos copiado anteriormente>**_  
EOF

![[hash-00-header 3.png]]
actualizar grub para que aplique las modificaciones que hemos realizado.
**root@orion:/#** update-grub

![[update-grub.png]]
Finalmente, para evitar que nos pida también el usuario y contraseña al **acceder** al **Sistema Operativo** editamos el archivo **/etc/grub.d/10_linux** :











