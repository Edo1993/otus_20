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
    link/ether 08:00:27:79:d1:69 brd ff:ff:ff:ff:ff:ff
    inet 10.10.10.1/24 brd 10.10.10.255 scope global noprefixroute eth1.10
       valid_lft forever preferred_lft forever
    inet6 fe80::a00:27ff:fe79:d169/64 scope link 
       valid_lft forever preferred_lft forever
```
