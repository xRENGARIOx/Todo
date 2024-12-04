Para el proyecto B
La red asignada es 192.168.20.0, es una clase C
Los hosts, deben tener asignadas IP's de esta red a partir de la vigesima primera IP disponible.
EL gw de cada una es la primera (1a) IP disponible


| Dispositivo | Interfaz                         | IP/Mask                                               | Gateway (si es que se requiere) | VLAN           |
| ----------- | -------------------------------- | ----------------------------------------------------- | ------------------------------- | -------------- |
| PrA1        | -                                | 192.168.10.11/24                                      | 192.168.10.1                    | 10             |
| PrA2        | -                                | 192.168.10.12/24                                      | 192.168.10.1                    | 10             |
| PrB1        | -                                | 192.168.20.21/24                                      | 192.168.20.1                    | 20             |
| PrB2        | -                                | 192.168.20.22/24                                      | 192.168.20.1                    | 20             |
| SA1         | VLAN99                           | 192.168.99.2/24                                       | 192.168.99.1                    |                |
| SA2         | VLAN99                           | 192.168.99.3/24                                       | 192.168.99.1                    |                |
| SD1         | VLAN99                           | 192.168.99.4/24                                       | 192.168.99.1                    |                |
| R1          | Gi0/0.10<br>Gi0/0.20<br>Gi0/0.99 | 192.168.10.1/24<br>192.168.20.1/24<br>192.168.99.1/24 |                                 | 10<br>20<br>99 |
| Adm1        | -                                | 192.168.99.10/24                                      | 192.168.99.1                    | 99             |
| Prn1        | -                                | 192.168.99.11/24                                      | 192.168.99.1                    | 99             |

Tabla de asignacion de VLANS

| VLAN | Nombre     | Tipo   | Puertos                                                                     | Device          |
| ---- | ---------- | ------ | --------------------------------------------------------------------------- | --------------- |
| 10   | Proyecto A | Datos  | Fa0/5-10<br>Fa0/11                                                          | SA1, SA2<br>SD1 |
| 20   | Proyecto B | Datos  | Fa0/11-15<br>Fa0/12                                                         | SA1, SA2<br>SD1 |
| 99   | Management | Admin  | Fa0/16-20                                                                   | SA1, SA2, SD1   |
| 999  | ParkingLot | Datos  | Fa0/1-4                                                                     | SA1, SA2        |
| 1000 | Nativa     | Nativa | Fa0/21-24, Gi0/1-2<br>Fa0/1-10,13-15, 21-24, Gi0/1-2<br>(Todos los Gigabit) | SA1, SA2<br>SD1 |

# SA1
```ciscoIOS
!Configuracion basicas de un switch
!Lab Redes II
!
enable
conf t
!Asignar un nombre distintivo al dispositivo
hostname SA1
!Asignar un dominio de trabajo
ip domain-name s8.lrc2.uaz.mx
!Deshabilitar la resolucion de nombres
no ip domain-lookup
service password-encryption
!Diseniar un banner o una etiqueta de presentacion
banner motd $
[+]-----------------------[+]
 | LabRedes2 - CCNAv7 SRWE |
 | Ejercicio M3 y M4       |
 | 5 de Semptiembre de 2024|
 | SA1                     |
 | Acceso restringido !!!  |
[+]-----------------------[+]
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


vlan 10
name ProyectoA
vlan 20
name ProyectoB
vlan 99
name Management
vlan 999
name ParkingLot
vlan 1000
name Native


interface range fa0/5-10
switchport mode access
switchport access vlan 10

interface range fa0/11-15
switchport mode access
switchport access vlan 20

interface range fa0/16-20
switchport mode access
switchport access vlan 99

interface range fa0/1-4
switchport mode access
switchport access vlan 999
shutdown

interface range fa0/5-10
switchport mode access
switchport access vlan 10

interface range fa0/21-24, gi0/1-2
switchport mode trunk
switchport trunk native vlan 1000
switchport trunk allowed vlan 10,20,99

interface vlan99
ip address 192.168.99.2 255.255.255.0
no shutdown
exit

ip default-gateway 192.168.99.1

```

```
end
copy run start
sh vlan brief


```

# SA2

```ciscoIOS
!Configuracion basicas de un switch
!Lab Redes II
!
enable
conf t
!Asignar un nombre distintivo al dispositivo
hostname SA2
!Asignar un dominio de trabajo
ip domain-name s8.lrc2.uaz.mx
!Deshabilitar la resolucion de nombres
no ip domain-lookup
service password-encryption
!Diseniar un banner o una etiqueta de presentacion
banner motd $
[+]-----------------------[+]
 | LabRedes2 - CCNAv7 SRWE |
 | Ejercicio M3 y M4       |
 | 5 de Semptiembre de 2024|
 | SA2                     |
 | Acceso restringido !!!  |
[+]-----------------------[+]
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


vlan 10
name ProyectoA
vlan 20
name ProyectoB
vlan 99
name Management
vlan 999
name ParkingLot
vlan 1000
name Native


interface range fa0/5-10
switchport mode access
switchport access vlan 10

interface range fa0/11-15
switchport mode access
switchport access vlan 20

interface range fa0/16-20
switchport mode access
switchport access vlan 99

interface range fa0/1-4
switchport mode access
switchport access vlan 999
shutdown

interface range fa0/5-10
switchport mode access
switchport access vlan 10

interface range fa0/21-24, gi0/1-2
no switchport mode trunk
switchport trunk native vlan 1000
switchport trunk allowed vlan 10,20,99
switchport access vlan 999

interface vlan99
ip address 192.168.99.3 255.255.255.0
no shutdown
exit

ip default-gateway 192.168.99.1
```



# SD1
```ciscoIOS
!Configuracion basicas de un switch
!Lab Redes II
!
enable
conf t
!Asignar un nombre distintivo al dispositivo
hostname SD1
!Asignar un dominio de trabajo
ip domain-name s8.lrc2.uaz.mx
!Deshabilitar la resolucion de nombres
no ip domain-lookup
service password-encryption
!Diseniar un banner o una etiqueta de presentacion
banner motd $
[+]-----------------------[+]
 | LabRedes2 - CCNAv7 SRWE |
 | Ejercicio M3 y M4       |
 | 5 de Semptiembre de 2024|
 | SD1                     |
 | Acceso restringido !!!  |
[+]-----------------------[+]
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


vlan 10
name ProyectoA
vlan 20
name ProyectoB
vlan 99
name Management
vlan 999
name ParkingLot
vlan 1000
name Native



interface fa0/11
switchport mode access
switchport access vlan 10

interface fa0/12
switchport mode access
switchport access vlan 20


interface range fa0/16-20
switchport mode access
switchport access vlan 99


interface range fa0/1-15,fa0/21-24,gi0/1-2
switchport mode trunk
switchport trunk native vlan 1000
switchport trunk allowed vlan 10,20,99

interface vlan99
ip address 192.168.99.4 255.255.255.0
no shutdown
exit

ip default-gateway 192.168.99.1
```

Maquinas de proyecto a
resultado esperado
vlans creada, troncales estan activos y debe haber un camino entre ellas.

resultado obtenido
se obitneen paquedes de 32 bytes de tamanio y en promedio les tomo 1 ms


Maquina b
4 paquetes enviados y 4 recibidos, promedio de 1 ms, exitoso

 

impresora
si, 4, 4, no hay perdida de paquetes, promedio de 1 a 4 ms

SD1 admin1
se perdio el primer ping, pero los demas ping fueron exitoso, 25% de perdida de paquetes.
lo mismo sucedio con los switches sa2 y sd1.
al segundo intento fueron exitosos todos

en caso de error, documentar que se verifico, ver la informacion y demas
el switch no tenia puertos asignados a la vlan 99



de la pc de pa1, a pb1, adm1 e impresora
no deberia haber conectividad puesto que las maquinas pertenence a vlan distintas, a dominios de difusion distintos, y aun no hay un dispositivo que permita interconexion entre vlans.

478d, se manda el icmp al router o gateway, pero como no hay, falla


```ciscoIOS
!route
!Configuracion basicas de un switch
!Lab Redes II
!
enable
conf t
!Asignar un nombre distintivo al dispositivo
hostname R1
!Asignar un dominio de trabajo
ip domain-name s8.lrc2.uaz.mx
!Deshabilitar la resolucion de nombres
no ip domain-lookup
service password-encryption
!Diseniar un banner o una etiqueta de presentacion
banner motd $
[+]-----------------------[+]
 | LabRedes2 - CCNAv7 SRWE |
 | Ejercicio M3 y M4       |
 | 5 de Semptiembre de 2024|
 | R1                      |
 | Acceso restringido !!!  |
[+]-----------------------[+]
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
line vty 0 4
login local
exec-timeout 15
! - Habilitar el acceso con ssh
transport input ssh

!buena practica darle la vlan que se va a usar
interface gi0/1.10
encapsulation dot1q 10
ip address 192.168.10.1 255.255.255.0

interface gi0/1.20
encapsulation dot1q 20
ip address 192.168.20.1 255.255.255.0

interface gi0/1.99
encapsulation dot1q 99
ip address 192.168.99.1 255.255.255.0

!es bueno pornel lo de la vlan nativa
interface gi0/1.1000
encapsulation dot1q 1000 native

int g0/1
no shutdown


```

Ahora el resultado esperado es que exista comunicacion entre todos los dispositivos

tablas d










# Tarea
show vlan brief
SA1:
SA1#show vlan brief

  

VLAN Name Status Ports

---- -------------------------------- --------- -------------------------------

1 default active

10 ProyectoA active Fa0/5, Fa0/6, Fa0/7, Fa0/8

Fa0/9, Fa0/10

20 ProyectoB active Fa0/11, Fa0/12, Fa0/13, Fa0/14

Fa0/15

99 Management active Fa0/16, Fa0/17, Fa0/18, Fa0/19

Fa0/20

999 ParkingLot active Fa0/1, Fa0/2, Fa0/3, Fa0/4

Fa0/21, Fa0/22, Fa0/23, Gig0/1

Gig0/2

1000 Native active

1002 fddi-default active

1003 token-ring-default active

1004 fddinet-default active

1005 trnet-default active

++++++++++++++++++++++++++++++++++++
SA2:
SA2#show vlan brief

  

VLAN Name Status Ports

---- -------------------------------- --------- -------------------------------

1 default active

10 ProyectoA active Fa0/5, Fa0/6, Fa0/7, Fa0/8

Fa0/9, Fa0/10

20 ProyectoB active Fa0/11, Fa0/12, Fa0/13, Fa0/14

Fa0/15

99 Management active Fa0/16, Fa0/17, Fa0/18, Fa0/19

Fa0/20

999 ParkingLot active Fa0/1, Fa0/2, Fa0/3, Fa0/4

Fa0/21, Fa0/22, Fa0/23, Gig0/1

Gig0/2

1000 Native active

1002 fddi-default active

1003 token-ring-default active

1004 fddinet-default active

1005 trnet-default active

++++++++++++++++++++++++++++++++++++
SD1:
SD1#show vlan brief

  

VLAN Name Status Ports

---- -------------------------------- --------- -------------------------------

1 default active Fa0/3, Fa0/4, Fa0/5, Fa0/6

Fa0/7, Fa0/8, Fa0/9, Fa0/10

Fa0/13, Fa0/14, Fa0/15, Fa0/21

Fa0/22, Fa0/23, Fa0/24, Gig0/2

10 ProyectoA active Fa0/11

20 ProyectoB active Fa0/12

99 Management active Fa0/16, Fa0/17, Fa0/18, Fa0/19

Fa0/20

999 ParkingLot active

1000 Native active

1002 fddi-default active

1003 token-ring-default active

1004 fddinet-default active

1005 trnet-default active

-------
++++++++++++++++++++++++++++++++++++++++++++++++++++++++++

show interface trunk
SA1:
SA1#show interface trunk

Port Mode Encapsulation Status Native vlan

Fa0/24 auto n-802.1q trunking 1000

  

Port Vlans allowed on trunk

Fa0/24 10,20,99

  

Port Vlans allowed and active in management domain

Fa0/24 10,20,99

  

Port Vlans in spanning tree forwarding state and not pruned

Fa0/24 10,20,99
++++++++++++++++++++++++++++++++++++

SA2:
SA2#show interface trunk

Port Mode Encapsulation Status Native vlan

Fa0/24 auto n-802.1q trunking 1000

  

Port Vlans allowed on trunk

Fa0/24 10,20,99

  

Port Vlans allowed and active in management domain

Fa0/24 10,20,99

  

Port Vlans in spanning tree forwarding state and not pruned

Fa0/24 10,20,99

++++++++++++++++++++++++++++++++++++

SD1:
SD1#show interface trunk

Port Mode Encapsulation Status Native vlan

Fa0/1 on 802.1q trunking 1000

Fa0/2 on 802.1q trunking 1000

Gig0/1 on 802.1q trunking 1000

  

Port Vlans allowed on trunk

Fa0/1 10,20,99

Fa0/2 10,20,99

Gig0/1 10,20,99

  

Port Vlans allowed and active in management domain

Fa0/1 10,20,99

Fa0/2 10,20,99

Gig0/1 10,20,99

  

Port Vlans in spanning tree forwarding state and not pruned

Fa0/1 10,20,99

Fa0/2 10,20,99

Gig0/1 10,20,99

++++++++++++++++++++++++++++++++++++++++++++++++++++++++++
show interface fa0/1 switchport
SA1:
SA1#show interface fa0/1 switchport

Name: Fa0/1

Switchport: Enabled

Administrative Mode: static access

Operational Mode: down

Administrative Trunking Encapsulation: dot1q

Operational Trunking Encapsulation: native

Negotiation of Trunking: Off

Access Mode VLAN: 999 (ParkingLot)

Trunking Native Mode VLAN: 1 (default)

Voice VLAN: none

Administrative private-vlan host-association: none

Administrative private-vlan mapping: none

Administrative private-vlan trunk native VLAN: none

Administrative private-vlan trunk encapsulation: dot1q

Administrative private-vlan trunk normal VLANs: none

Administrative private-vlan trunk private VLANs: none

Operational private-vlan: none

Trunking VLANs Enabled: All

Pruning VLANs Enabled: 2-1001

Capture Mode Disabled

Capture VLANs Allowed: ALL

Protected: false

Unknown unicast blocked: disabled

Unknown multicast blocked: disabled

Appliance trust: none

++++++++++++++++++++++++++++++++++++

SA2:
SA2#show interface fa0/1 switchport

Name: Fa0/1

Switchport: Enabled

Administrative Mode: static access

Operational Mode: down

Administrative Trunking Encapsulation: dot1q

Operational Trunking Encapsulation: native

Negotiation of Trunking: Off

Access Mode VLAN: 999 (ParkingLot)

Trunking Native Mode VLAN: 1 (default)

Voice VLAN: none

Administrative private-vlan host-association: none

Administrative private-vlan mapping: none

Administrative private-vlan trunk native VLAN: none

Administrative private-vlan trunk encapsulation: dot1q

Administrative private-vlan trunk normal VLANs: none

Administrative private-vlan trunk private VLANs: none

Operational private-vlan: none

Trunking VLANs Enabled: All

Pruning VLANs Enabled: 2-1001

Capture Mode Disabled

Capture VLANs Allowed: ALL

Protected: false

Unknown unicast blocked: disabled

Unknown multicast blocked: disabled

Appliance trust: none

++++++++++++++++++++++++++++++++++++

SD1:
SD1#show interface fa0/1 switchport

Name: Fa0/1

Switchport: Enabled

Administrative Mode: trunk

Operational Mode: trunk

Administrative Trunking Encapsulation: dot1q

Operational Trunking Encapsulation: dot1q

Negotiation of Trunking: On

Access Mode VLAN: 1 (default)

Trunking Native Mode VLAN: 1000 (Native)

Voice VLAN: none

Administrative private-vlan host-association: none

Administrative private-vlan mapping: none

Administrative private-vlan trunk native VLAN: none

Administrative private-vlan trunk encapsulation: dot1q

Administrative private-vlan trunk normal VLANs: none

Administrative private-vlan trunk private VLANs: none

Operational private-vlan: none

Trunking VLANs Enabled: 10,20,99

Pruning VLANs Enabled: 2-1001

Capture Mode Disabled

Capture VLANs Allowed: ALL

Protected: false

Unknown unicast blocked: disabled

Unknown multicast blocked: disabled

Appliance trust: none


OS300824


EvidenciaM3yM4_42102866_090624

---

# Configurando el Switch de Capa 3


```ciscoIOS
!Configuracion basicas de un switch
!Lab Redes II
!
enable
conf t
!Asignar un nombre distintivo al dispositivo
hostname SC1
!Asignar un dominio de trabajo
ip domain-name s10.lrc2.uaz.mx
!Deshabilitar la resolucion de nombres
no ip domain-lookup
service password-encryption
!Diseniar un banner o una etiqueta de presentacion
banner motd $
[+]-----------------------[+]
 | LabRedes2 - CCNAv7 SRWE |
 | Ejercicio M3 y M4       |
 | 10 de Septiembre de 2024|
 | SC1                     |
 | Acceso restringido !!!  |
[+]-----------------------[+]
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


vlan 10
name ProyectoA
vlan 20
name ProyectoB
vlan 99
name Management
vlan 999
name ParkingLot
vlan 1000
name Native

interface gi1/0/10
switchport mode access
switchport acces vlan 10

interface gi1/0/20
switchport mode access
switchport acces vlan 20

interface gi1/0/24
switchport mode access
switchport acces vlan 99

interface vlan10
ip address 192.168.10.1 255.255.255.0
interface vlan20
ip address 192.168.20.1 255.255.255.0
interface vlan99
ip address 192.168.99.1 255.255.255.0

exit
ip routing

```
