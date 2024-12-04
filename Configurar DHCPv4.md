
```ciscoIOS
!Config de DHCP como tal
!Vlan azul
ip dhcp excluded-address 192.168.20.1 192.168.20.99
ip dhcp excluded-address 192.168.20.201 192.168.20.254

!vlan verde
ip dhcp excluded-address 192.168.10.1 192.168.10.49
ip dhcp excluded-address 192.168.10.151 192.168.10.254

ip dhcp pool VLAN-AZUL
network 192.168.20.0 255.255.255.0
domain-name azul.m11.uaz.mx
dns-server 8.8.8.8
default-router 192.168.20.1

ip dhcp pool VLAN-VERDE
network 192.168.10.0 255.255.255.0
domain-name verde.m11.uaz.mx
dns-server 8.8.8.8
default-router 192.168.10.1
```


```ciscoIOS
!Vlan azul
ip dhcp excluded-address 192.168.1.193 192.168.1.201
ip dhcp excluded-address 192.168.1.213 192.168.1.254

!vlan roja
ip dhcp excluded-address 192.168.1.177 192.168.1.185

ip dhcp pool NetAzul
network 192.168.1.192 255.255.255.192
domain-name azul.lrc2.uaz.mx
dns-server 209.165.100.130
default-router 192.168.1.193

ip dhcp pool NetRojo
network 192.168.1.176 255.255.255.240
domain-name rojo.lrc2.uaz.mx
dns-server 209.165.100.130
default-router 192.168.1.177
```

# Borra las pool
```
no ip dhcp excluded-address 192.168.1.1 192.168.1.153
no ip dhcp pool LOCAL_NET_POOL
```

# Conf DHCP Relay -- IP del DHCP
```ciscoIOS
interface gi0/1.64
ip helper-address 192.168.100.254

interface gi0/1.128
ip helper-address 192.168.100.254

interface gi0/1.160
ip helper-address 192.168.100.254

```

