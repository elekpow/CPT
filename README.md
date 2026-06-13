# CPT
CiscoPacketTracer

**Настройка VLAN**

                    Интернет
                       │
                       │ 10.0.0.2
                       │
                ┌──────┴──────┐
                │ 10.0.0.1/30 │
                │   Роутер    │
                │ 192.168.10.1│
                │ 192.168.20.1│
                └──────┬──────┘
                       │ Trunk (VLAN 10,20)
                       │
                ┌──────┴──────┐
                │ Коммутатор  │
                └──────┬──────┘
         ┌─────────────┼─────────────┐
         │                           │
    ┌────┴────┐                 ┌────┴────┐
    │ Fa0/1   │                 │ Fa0/4   │
    │ VLAN 10 │                 │ VLAN 20 │
    │ PC0     │                 │ PC1     │
    └─────────┘                 └─────────┘

![shema-1.JPG](https://github.com/elekpow/CPT/blob/main/img/shema-1.JPG)

1 настройка роутера

```
enable
configure terminal
interface gigabitEthernet 0/0/0.10
encapsulation dot1Q 10
ip address 192.168.10.1 255.255.255.0
no shutdown
exit
interface gigabitEthernet 0/0/0.20
encapsulation dot1Q 20
ip address 192.168.20.1 255.255.255.0
no shutdown
exit
interface gigabitEthernet 0/0/0
no shutdown
exit
interface gigabitEthernet 0/0/1
ip address 10.0.0.1 255.255.255.252
no shutdown
exit
ip dhcp pool VLAN10_POOL
network 192.168.10.0 255.255.255.0
default-router 192.168.10.1
dns-server 8.8.8.8
exit
ip dhcp pool VLAN20_POOL
network 192.168.20.0 255.255.255.0
default-router 192.168.20.1
dns-server 8.8.8.8
exit
ip dhcp excluded-address 192.168.10.1
ip dhcp excluded-address 192.168.20.1
end
write memory
```

2 Настройка коммутатора

```
enable
configure terminal
vlan 10
name VLAN10
exit
vlan 20
name VLAN20
exit
interface range fastEthernet 0/1-3
switchport mode access
switchport access vlan 10
no shutdown
exit
interface range fastEthernet 0/4-6
switchport mode access
switchport access vlan 20
no shutdown
exit
interface gigabitEthernet 0/1
switchport mode trunk
switchport trunk allowed vlan 10,20
no shutdown
exit
end
write memory
```

