Redundancia de gateway

```ciscoIOS
# Configurar el standby en la azul
```ciscoIOS
conf t
interface gi0/1.192
standby version 2
standby 192 ip 192.168.1.193
standby 192 priority 200
standby 192 preempt
end

```

```ciscoIOS
conf t
interface gi0/1.192
standby version 2
standby 192 ip 192.168.1.193
standby 192 preempt
end

```
