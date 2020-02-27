# otus_20
# Сетевые пакеты. VLAN. LACP

Домашнее задание.

Строим бонды и вланы:
в Office1 в тестовой подсети появляется сервера с доп интерфесами и адресами в internal сети testLAN
- testClient1 - 10.10.10.254
- testClient2 - 10.10.10.254
- testServer1- 10.10.10.1
- testServer2- 10.10.10.1

Развести вланами:
testClient1 <-> testServer1
testClient2 <-> testServer2

Между centralRouter и inetRouter: "пробросить" 2 линка (общая inernal сеть) и объединить их в бонд проверить работу c отключением интерфейсов.

Для сдачи - вагрант файл с требуемой конфигурацией, разворачиваться конфигурация должна через ансибл.

__________________________________________________________________________________________________________________________


C centralRouter не пингуется inetRouter

```
[root@centralRouter vagrant]# ip r
default via 192.168.255.1 dev team0 proto static metric 350 
10.0.2.0/24 dev eth0 proto kernel scope link src 10.0.2.15 metric 100 
10.10.10.0/24 dev VLAN100 proto kernel scope link src 10.10.10.10 metric 400 
10.10.10.0/24 dev VLAN101 proto kernel scope link src 10.10.10.11 metric 401 
192.168.255.0/24 dev team0 proto kernel scope link src 192.168.255.2 metric 350 
[root@centralRouter vagrant]# ip link
1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN mode DEFAULT group default qlen 1000
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
2: eth0: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc pfifo_fast state UP mode DEFAULT group default qlen 1000
    link/ether 52:54:00:8a:fe:e6 brd ff:ff:ff:ff:ff:ff
3: eth1: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc pfifo_fast master team0 state UP mode DEFAULT group default qlen 1000
    link/ether 08:00:27:af:78:fc brd ff:ff:ff:ff:ff:ff
4: eth2: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc pfifo_fast master team0 state UP mode DEFAULT group default qlen 1000
    link/ether 08:00:27:af:78:fc brd ff:ff:ff:ff:ff:ff
5: eth3: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc pfifo_fast state UP mode DEFAULT group default qlen 1000
    link/ether 08:00:27:68:ae:d2 brd ff:ff:ff:ff:ff:ff
7: team0: <NO-CARRIER,BROADCAST,MULTICAST,UP> mtu 1500 qdisc noqueue state DOWN mode DEFAULT group default qlen 1000
    link/ether 08:00:27:af:78:fc brd ff:ff:ff:ff:ff:ff
8: VLAN100@eth3: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc noqueue state UP mode DEFAULT group default qlen 1000
    link/ether 08:00:27:68:ae:d2 brd ff:ff:ff:ff:ff:ff
9: VLAN101@eth3: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc noqueue state UP mode DEFAULT group default qlen 1000
    link/ether 08:00:27:68:ae:d2 brd ff:ff:ff:ff:ff:ff
[root@centralRouter vagrant]# ping -c 4 192.168.255.1
PING 192.168.255.1 (192.168.255.1) 56(84) bytes of data.
From 192.168.255.2 icmp_seq=1 Destination Host Unreachable
From 192.168.255.2 icmp_seq=2 Destination Host Unreachable
From 192.168.255.2 icmp_seq=3 Destination Host Unreachable
From 192.168.255.2 icmp_seq=4 Destination Host Unreachable

--- 192.168.255.1 ping statistics ---
4 packets transmitted, 0 received, +4 errors, 100% packet loss, time 3005ms
pipe 3
```

testServer2 - интернета нет
```
[root@testServer2 vagrant]# ip r
default via 10.10.10.11 dev VLAN101 proto static metric 400 
10.0.2.0/24 dev eth0 proto kernel scope link src 10.0.2.15 metric 100 
10.10.10.0/24 dev VLAN101 proto kernel scope link src 10.10.10.254 metric 400 
[root@testServer2 vagrant]# ping -c 4 otus.ru
PING otus.ru (80.87.192.10) 56(84) bytes of data.
From testServer2 (10.10.10.254) icmp_seq=1 Destination Host Unreachable
From testServer2 (10.10.10.254) icmp_seq=2 Destination Host Unreachable
From testServer2 (10.10.10.254) icmp_seq=3 Destination Host Unreachable
From testServer2 (10.10.10.254) icmp_seq=4 Destination Host Unreachable

--- otus.ru ping statistics ---
4 packets transmitted, 0 received, +4 errors, 100% packet loss, time 3000ms
pipe 4
```

testServer1 - интернета нет
```
[root@testServer1 vagrant]# ip r
default via 10.10.10.10 dev VLAN100 proto static metric 400 
10.0.2.0/24 dev eth0 proto kernel scope link src 10.0.2.15 metric 100 
10.10.10.0/24 dev VLAN100 proto kernel scope link src 10.10.10.254 metric 400 
[root@testServer1 vagrant]# ping -c 4 otus.ru
PING otus.ru (80.87.192.10) 56(84) bytes of data.

--- otus.ru ping statistics ---
4 packets transmitted, 0 received, 100% packet loss, time 2999ms
```
