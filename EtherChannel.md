La empresa solicito un agregado de enlace entre los switches de acceso y el switch de distribucion

No mas de 6 agregaciones o canales de puerto, y cada uno no puede tener mas de 8 puertos.
Misma velocidad, misma forma de transmision, pertenecer a la misma vlan.

# SA1:

```ciscoIOS
enable
show running-config | section interface
conf t
!pagp
int range gi0/1-2
channel-group 1 mode auto 
!on, desirable, auto
end

show ip int brief
```

cisco123
class123

# SD1:
```ciscoIOS
conf t
int range g1/0/1,g1/0/11
channel-group 1 mode desirable
show ip int brief

int range g1/0/1,g1/0/11
no switchport 

```

# Reparar errores profe
```ciscoIOS
no channel-group mode desirable
no acces vlan 666 asdasdasd
do show vlan brief


!SA1
conf t
int range g0/1-2
no channel-group 1 mode auto
no switchport trunk  sadsada



```

```ciscoIOS
int po1
switchport mode trunk
switchport trunk native vlan 1000
switchport access vlan 666
no shutdown
```

```ciscoIOS
conf t
int po1

show interface trunk

```


usando lacp
```
show run | section interface
conf t
int range gi1/0/2,gi1/0/12
channel-group 2 mode active
do show vlan brief
int po2
switchport mode trunk
switchport access vlan 666
switchport trunk native vlan 1000
switchport trunk allowed vlan 32,64,128,1000
no shutdown

do show vlan brief

```


SA2:
```
conf t
int range gi0/1-2
channel-group 2 mode passive
do show vlan brief
int po2
switchport mode trunk
switchport access vlan 666
switchport trunk native vlan 1000
switchport trunk allowed vlan 32,64,128,1000
no shutdown
end
show vlan brief
```
\

troncal 
permitir las vlan 
parte de la 666




```ciscoIOS
interface range fa0/1 - 2
channel-group 1 mode active
exit
```