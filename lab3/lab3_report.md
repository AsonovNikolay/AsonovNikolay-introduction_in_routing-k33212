University: ITMO University

Faculty: FICT

Course: Introduction in routing

Year: 2022/2023

Group: K33212

Author: Asonov Nikolai

Lab: Lab3

Date of create: 08.01.2023

Date of finished:

# Отчёт по лабораторной работе №3 "Эмуляция распределенной корпоративной сети связи, настройка OSPF и MPLS, организация первого EoMPLS"

***Цель:*** Изучить протоколы OSPF и MPLS, механизмы организации EoMPLS.

## Ход работы ##
Текст файла lab3.clab.yml для развертывания сети



Настроим каждое устройство в нашей сети:

1) Роутер R01.SPB

'''
/interface bridge
add name=EoMPLS
add name=Lo0
/interface vpls
add cisco-style=yes cisco-style-id=666 disabled=no l2mtu=1500 mac-address=\
    02:BF:EE:4D:EF:F1 name=EoMPLS_VPLS remote-peer=150.150.150.6
/interface wireless security-profiles
set [ find default=yes ] supplicant-identity=MikroTik
/routing ospf instance
set [ find default=yes ] name=ospf0 router-id=150.150.150.1
/interface bridge port
add bridge=EoMPLS interface=ether4
add bridge=EoMPLS interface=EoMPLS_VPLS
/ip address
add address=172.31.255.30/30 interface=ether1 network=172.31.255.28
add address=150.150.1.1/30 interface=ether3 network=150.150.1.0
add address=150.150.2.1/30 interface=ether2 network=150.150.2.0
add address=192.168.150.1/24 interface=ether4 network=192.168.150.0
add address=150.150.150.1 interface=Lo0 network=150.150.150.1
/ip dhcp-client
add disabled=no interface=ether1
/ip route
add distance=1 dst-address=192.168.150.0/24 gateway=ether4
add distance=1 dst-address=192.168.20.0/24 gateway=150.150.150.6
/mpls ldp
set enabled=yes transport-address=150.150.150.1
/mpls ldp interface
add interface=ether2
add interface=ether3
/routing ospf network
add area=backbone
/system identity
set name=R01.SPB
'''

2) Роутер R01.MSK

'''
/interface bridge
add name=Lo0
/interface wireless security-profiles
set [ find default=yes ] supplicant-identity=MikroTik
/routing ospf instance
set [ find default=yes ] name=ospf0 router-id=150.150.150.2
/ip address
add address=172.31.255.30/30 interface=ether1 network=172.31.255.28
add address=150.150.1.2/30 interface=ether3 network=150.150.1.0
add address=150.150.3.1/30 interface=ether4 network=150.150.3.0
add address=150.150.150.2 interface=Lo0 network=150.150.150.2
/ip dhcp-client
add disabled=no interface=ether1
/mpls ldp
set enabled=yes transport-address=150.150.150.2
/mpls ldp interface
add interface=ether3
add interface=ether4
/routing ospf network
add area=backbone
/system identity
set name=R01.MSK
'''

3) Роутер R01.HKI
'''
/interface bridge
add name=Lo0
/interface wireless security-profiles
set [ find default=yes ] supplicant-identity=MikroTik
/routing ospf instance
set [ find default=yes ] name=ospf0 router-id=150.150.150.3
/ip address
add address=172.31.255.30/30 interface=ether1 network=172.31.255.28
add address=150.150.2.2/30 interface=ether2 network=150.150.2.0
add address=150.150.4.1/30 interface=ether3 network=150.150.4.0
add address=150.150.5.1/30 interface=ether4 network=150.150.5.0
add address=150.150.150.3 interface=Lo0 network=150.150.150.3
/ip dhcp-client
add disabled=no interface=ether1
/mpls ldp
set enabled=yes transport-address=150.150.150.3
/mpls ldp interface
add interface=ether2
add interface=ether3
add interface=ether4
/routing ospf network
add area=backbone
/system identity
set name=R01.HKI
'''


4) Роутер R01.LND
'''
/interface bridge
add name=Lo0
/interface wireless security-profiles
set [ find default=yes ] supplicant-identity=MikroTik
/routing ospf instance
set [ find default=yes ] name=ospf0 router-id=150.150.150.5
/ip address
add address=172.31.255.30/30 interface=ether1 network=172.31.255.28
add address=150.150.6.1/30 interface=ether3 network=150.150.6.0
add address=150.150.5.2/30 interface=ether4 network=150.150.5.0
add address=150.150.150.5 interface=Lo0 network=150.150.150.5
/ip dhcp-client
add disabled=no interface=ether1
/mpls ldp
set enabled=yes transport-address=150.150.150.5
/mpls ldp interface
add interface=ether3
add interface=ether4
/routing ospf network
add area=backbone
/system identity
set name=R01.LND
'''

5) Роутер R01.LBN
'''
/interface bridge
add name=Lo0
/interface wireless security-profiles
set [ find default=yes ] supplicant-identity=MikroTik
/routing ospf instance
set [ find default=yes ] name=ospf0 router-id=150.150.150.4
/ip address
add address=172.31.255.30/30 interface=ether1 network=172.31.255.28
add address=150.150.7.1/30 interface=ether2 network=150.150.7.0
add address=150.150.4.2/30 interface=ether3 network=150.150.4.0
add address=150.150.3.2/30 interface=ether4 network=150.150.3.0
add address=150.150.150.4 interface=Lo0 network=150.150.150.4
/ip dhcp-client
add disabled=no interface=ether1
/mpls ldp
set enabled=yes transport-address=150.150.150.4
/mpls ldp interface
add interface=ether2
add interface=ether3
add interface=ether4
/routing ospf network
add area=backbone
/system identity
set name=R01.LBN
'''


6) Роутер R01.NY

'''
/interface bridge
add name=EoMPLS
add name=Lo0
/interface vpls
add cisco-style=yes cisco-style-id=666 disabled=no l2mtu=1500 mac-address=\
    02:25:B9:5B:6B:A3 name=EoMPLS_VPLS remote-peer=150.150.150.1
/interface wireless security-profiles
set [ find default=yes ] supplicant-identity=MikroTik
/routing ospf instance
set [ find default=yes ] name=ospf0 router-id=150.150.150.6
/interface bridge port
add bridge=EoMPLS interface=ether4
add bridge=EoMPLS interface=EoMPLS_VPLS
/ip address
add address=172.31.255.30/30 interface=ether1 network=172.31.255.28
add address=150.150.7.2/30 interface=ether2 network=150.150.7.0
add address=150.150.6.2/30 interface=ether3 network=150.150.6.0
add address=192.168.20.1/24 interface=ether4 network=192.168.20.0
add address=150.150.150.6 interface=Lo0 network=150.150.150.6
/ip dhcp-client
add disabled=no interface=ether1
/ip route
add distance=1 dst-address=192.168.150.0/24 gateway=150.150.150.1
add distance=1 dst-address=192.168.20.0/24 gateway=ether4
/mpls ldp
set enabled=yes transport-address=150.150.150.6
/mpls ldp interface
add interface=ether2
add interface=ether3
/routing ospf network
add area=backbone
/system identity
set name=R01.NY
'''



7) Сервер SGI_Prism
'''
/interface wireless security-profiles
set [ find default=yes ] supplicant-identity=MikroTik
/ip address
add address=172.31.255.30/30 interface=ether1 network=172.31.255.28
add address=192.168.20.250/24 interface=ether4 network=192.168.20.0
/ip dhcp-client
add disabled=no interface=ether1
/ip route
add distance=1 gateway=192.168.20.1
/system identity
set name=SGI_Prism
'''

8) Компьютер PC1
'''
/interface wireless security-profiles
set [ find default=yes ] supplicant-identity=MikroTik
/ip address
add address=172.31.255.30/30 interface=ether1 network=172.31.255.28
add address=192.168.150.250/24 interface=ether4 network=192.168.150.0
/ip dhcp-client
add disabled=no interface=ether1
/ip route
add distance=1 gateway=192.168.150.1
/system identity
set name=PC1
'''

После этого проверим таблицу маршрутизации на роутере:


Также проверим информацию об mpls отметках с помощью команды "tool trace ip-R01.SPB":



## Вывод ##
В ходе выполнения лабораторной работы №3, мы изучили и научились использовать OSPF, MPLS и EoMPLS.В итоге мы получили рабочую связь для компании "RogaIKopita Games".
