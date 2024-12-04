
```ciscoIOS
vlan 176
name ROJA
vlan 192
name AZUL
vlan 1000
name NATIVE

interface fa0/1
switchport mode access
switchport access vlan 192

interface fa0/2
switchport mode access
switchport access vlan 176

interface range fa0/20-24
switchport mode trunk
switchport trunk native vlan 1000
switchport trunk allowed vlan 176,192,1000


interface vlan 176
ip address 192.168.1.181 255.255.255.240

exit
ip default-gateway 192.168.1.177
```
