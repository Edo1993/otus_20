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

Для проверки ```vagrant up```

Входим на *testServer1*

```
vagrant ssh testServer1
sudo su
ip -c a show eth1.10 
```
Ответ
```
4: eth1.10@eth1: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc noqueue state UP group default qlen 1000
    link/ether 08:00:27:3c:1a:b6 brd ff:ff:ff:ff:ff:ff
    inet 10.10.10.1/24 brd 10.10.10.255 scope global noprefixroute eth1.10
       valid_lft forever preferred_lft forever
    inet6 fe80::a00:27ff:fe3c:1ab6/64 scope link 
       valid_lft forever preferred_lft forever
```
![Img_alt](https://github.com/Edo1993/otus_20/blob/master/img/201.png)

Входим на *testClient1*

```
vagrant ssh testClient1
sudo su
ip -c a show eth1.10
```
Ответ
```
4: eth1.10@eth1: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc noqueue state UP group default qlen 1000
    link/ether 08:00:27:69:d4:a5 brd ff:ff:ff:ff:ff:ff
    inet 10.10.10.254/24 brd 10.10.10.255 scope global noprefixroute eth1.10
       valid_lft forever preferred_lft forever
    inet6 fe80::a00:27ff:fe69:d4a5/64 scope link 
       valid_lft forever preferred_lft forever
```
Далее
```
ping -c 5 10.10.10.1
```
Ответ
```
PING 10.10.10.1 (10.10.10.1) 56(84) bytes of data.
64 bytes from 10.10.10.1: icmp_seq=1 ttl=64 time=1.49 ms
64 bytes from 10.10.10.1: icmp_seq=2 ttl=64 time=0.751 ms
64 bytes from 10.10.10.1: icmp_seq=3 ttl=64 time=0.776 ms
64 bytes from 10.10.10.1: icmp_seq=4 ttl=64 time=0.795 ms
64 bytes from 10.10.10.1: icmp_seq=5 ttl=64 time=0.762 ms

--- 10.10.10.1 ping statistics ---
5 packets transmitted, 5 received, 0% packet loss, time 4005ms
rtt min/avg/max/mdev = 0.751/0.915/1.493/0.290 ms
```
Далее
```
ip neigh
```
Ответ
```
10.0.2.2 dev eth0 lladdr 52:54:00:12:35:02 REACHABLE
10.10.10.1 dev eth1.10 lladdr 08:00:27:3c:1a:b6 REACHABLE
```
![Img_alt](https://github.com/Edo1993/otus_20/blob/master/img/202.png)

Проверяем на *testServer1*
```
ip neigh
```
Ответ
```
10.10.10.254 dev eth1.10 lladdr 08:00:27:69:d4:a5 REACHABLE
10.0.2.2 dev eth0 lladdr 52:54:00:12:35:02 REACHABLE
```
![Img_alt](https://github.com/Edo1993/otus_20/blob/master/img/203.png)

____________________________________________

Bond

Смотрим inetRouter.
```
vagrant ssh inetRouter
sudo su
more /proc/net/bonding/bond0
```
Ответ
```
Ethernet Channel Bonding Driver: v3.7.1 (April 27, 2011)

Bonding Mode: fault-tolerance (active-backup)
Primary Slave: None
Currently Active Slave: eth1
MII Status: up
MII Polling Interval (ms): 1
Up Delay (ms): 0
Down Delay (ms): 0

Slave Interface: eth1
MII Status: up
Speed: 1000 Mbps
Duplex: full
Link Failure Count: 0
Permanent HW addr: 08:00:27:c6:b5:c1
Slave queue ID: 0

Slave Interface: eth2
MII Status: up
Speed: 1000 Mbps
Duplex: full
Link Failure Count: 0
Permanent HW addr: 08:00:27:53:7c:bc
Slave queue ID: 0
```
![Img_alt](https://github.com/Edo1993/otus_20/blob/master/img/205.png)

Смотрим centralRouter.
```
vagrant ssh centralRouter
sudo su
more /proc/net/bonding/bond0
```
Ответ
```
Ethernet Channel Bonding Driver: v3.7.1 (April 27, 2011)

Bonding Mode: fault-tolerance (active-backup)
Primary Slave: None
Currently Active Slave: eth1
MII Status: up
MII Polling Interval (ms): 1
Up Delay (ms): 0
Down Delay (ms): 0

Slave Interface: eth1
MII Status: up
Speed: 1000 Mbps
Duplex: full
Link Failure Count: 0
Permanent HW addr: 08:00:27:c8:5f:93
Slave queue ID: 0

Slave Interface: eth2
MII Status: up
Speed: 1000 Mbps
Duplex: full
Link Failure Count: 0
Permanent HW addr: 08:00:27:21:66:33
Slave queue ID: 0
```
![Img_alt](https://github.com/Edo1993/otus_20/blob/master/img/206.png)

Отключаем eth2.
```
ifdown eth2
```
Device 'eth2' successfully disconnected.
Проверяем состояние
```
cat /proc/net/bonding/bond0
```
Ответ
```
Ethernet Channel Bonding Driver: v3.7.1 (April 27, 2011)

Bonding Mode: fault-tolerance (active-backup)
Primary Slave: None
Currently Active Slave: eth1
MII Status: up
MII Polling Interval (ms): 1
Up Delay (ms): 0
Down Delay (ms): 0

Slave Interface: eth1
MII Status: up
Speed: 1000 Mbps
Duplex: full
Link Failure Count: 0
Permanent HW addr: 08:00:27:c8:5f:93
Slave queue ID: 0
```
![Img_alt](https://github.com/Edo1993/otus_20/blob/master/img/207.png)

После этого на inetRouter проверяем пинг
```
vagrant ssh inetRouter
sudo su
ping -c5 192.168.255.2
```
Ответ
```
PING 192.168.255.2 (192.168.255.2) 56(84) bytes of data.
64 bytes from 192.168.255.2: icmp_seq=1 ttl=64 time=0.051 ms
64 bytes from 192.168.255.2: icmp_seq=2 ttl=64 time=0.045 ms
64 bytes from 192.168.255.2: icmp_seq=3 ttl=64 time=0.028 ms
64 bytes from 192.168.255.2: icmp_seq=4 ttl=64 time=0.045 ms
64 bytes from 192.168.255.2: icmp_seq=5 ttl=64 time=0.047 ms

--- 192.168.255.2 ping statistics ---
5 packets transmitted, 5 received, 0% packet loss, time 3996ms
rtt min/avg/max/mdev = 0.028/0.043/0.051/0.008 ms
```
![Img_alt](https://github.com/Edo1993/otus_20/blob/master/img/204.png)
