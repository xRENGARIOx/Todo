
```ciscoIOS

!slack y dhcp

conf t
ipv6 dhcp pool VLAN20_SLDHCP
domain-name slvlan20.lrc2.uaz.mx
dns-server 2001:4860:4860::8888
do show ipv6 dhcp pool

interface gi0/0
ipv6 address fe80::1 link-local
ipv6 address 2001:db8:acad:14::1/64
ipv6 nd other-config-flag
ipv6 dhcp server VLAN20_SLDHCP
do show ipv6 int gi0/0
no shutdown


! manejado

end
conf t
ipv6 dhcp pool VLAN30_SFDHCP
domain-name sfvlan30.lrc2.uaz.mx
dns-server 2001:4860:4860::8844
addres prefix 2001:db8:acad:1e::/64

do show ipv6 dhcp pool

int gi0/2
ipv6 address fe80::1 link-local
ipv6 address 2001:db8:acad:1e::1/64
ipv6 nd managed-config-flag

ipv6 nd prefix default no-autoconfig
ipv6 dhcp server VLAN30_SFDHCP
no shutdown
```
