
```ciscoIOS
!Configuracion basicas de un switch
!Lab Redes II
!
enable
conf t
!Asignar un nombre distintivo al dispositivo
hostname Sw1
!Asignar un dominio de trabajo
ip domain-name lrc2.uaz.mx
!Deshabilitar la resolucion de nombres
no ip domain-lookup
!Diseniar un banner o una etiqueta de presentacion
banner motd $
[+]-------------------------[+]
 | LabRedes2 - CCNAv7 SRWE   |
 | Ejercicio Modulo 14       |
 | 14 de Noviembre de 2024   |
 | Sw1                       |
 | Mario Martinez 42102866   |
 | Acceso restringido !!!    |
[+]-------------------------[+]
$
!Crear una base de datos local para usuario
username admin secret cisco123
!Asignar una password al nivel privilegiado
enable secret class123
!Habilitar el acceso por SSH
! 1. Generar una llave para el cifrado de datos
crypto key generate rsa
1024
! 2. Habilitar el ssh version 2
ip ssh version 2
! 3. Configurar las lineas de acceso (admin), consola y terminales virtuales
line console 0
! - Se use la base de datos local para acceso
login local
! - Se sincronizen los mensajes a consola
loggin synchronous
! - Tiempo de actividad 
exec-timeout 15
! - Lineas virtuales (vty)
line vty 0 15
login local
exec-timeout 15
! - Habilitar el acceso con ssh
transport input ssh

```

# Config de SVI
```ciscoIOS
! - Configurar la SVI para acceso remoto
interface vlan 1
! - Asignar direccion IP
ip addres 192.168.2.10 255.255.255.0
! Se enciende la interfaz
no shutdown
exit
ip default-gateway 192.168.1.1
```
