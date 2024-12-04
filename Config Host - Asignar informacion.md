```bash
ifconfig <nombre> <direccion> netmask 255.255.255.0 up
ifconfig
route add default gw 192.168.1.1
netstat -rn
ifconfig <nombre> add 2001:db8:acad:1::10/64
route -A inet6 add default gw fe80::1 dev <nombre_tarjeta>
```
