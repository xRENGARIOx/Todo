
# IPv6 Unicast
```ciscoIOS
! - Habilitar el routing IPv6
ipv6 unicast-routing

```

# Configurar una interfaz
```ciscoIOS
! - Configurar la Interfaz Gigabit 0/1 porque es la interfaz del router que estamos utilizando
interface g0/1
ip addres 192.168.1.1 255.255.255.0
ipv6 address fe80::1 link-local
ipv6 address 2001:db8:acad:1::1/64
desc Enlace a S1
no shutdown
```

# Cambiar duplex y speed
```ciscoIOS
duplex half
speed 10

duplex full
```


# Troncal
```ciscoIOS
enable
conf t
interface fa0/24
switchport mode trunk 
switchport trunk native vlan 1
switchport trunk allowed vlan 10,20







interface range gi1/1/1-4
switchport mode trunk
switchport acces vlan 666
switchport trunk native vlan 1000
switchport trunk allowed vlan 32,64,128,1000
```

# Sub-Interfaces
```ciscoIOS
interface gi0/1.20
encapsulation dot1q 20
ip address 192.168.20.1 255.255.255.0
```
