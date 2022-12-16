University: [ITMO University](https://itmo.ru/ru/)

Faculty: [FICT](https://fict.itmo.ru)

Course: [Introduction in routing](https://github.com/itmo-ict-faculty/introduction-in-routing)

Year: 2022/2023

Group: K33212

Author: Asonov Nikolai

Lab: Lab1

Date of create: 14.10.2022

Date of finished: 

# Отчёт по лабораторной работе №1 "Установка ContainerLab и развертывание тестовой сети связи"

***Цель:*** ознакомиться с инструментом ContainerLab и методами работы с ним, изучить работу VLAN, IP адресации и т.д.

### Ход работы

1. Текст файла, который был использован для развертывания тестовой сети с расширением .yaml:

       name: lab1

        mgmt:
        network: statics
        ipv4_subnet: 172.20.20.0/24

       topology:
        nodes:
          R01.TEST:
            kind: vr-ros
            image: vrnetlab/vr-routeros:6.47.9
            mgmt_ipv4: 172.20.20.2`

          SW01.01.TEST:
            kind: vr-ros
            image: vrnetlab/vr-routeros:6.47.9
            mgmt_ipv4: 172.20.20.3

          SW02.01.TEST:
            kind: vr-ros
            image: vrnetlab/vr-routeros:6.47.9
            mgmt_ipv4: 172.20.20.4

          SW02.02.TEST:
            kind: vr-ros
            image: vrnetlab/vr-routeros:6.47.9
            mgmt_ipv4: 172.20.20.5

          PC1:
            kind: linux
            image: ubuntu:latest
            mgmt_ipv4: 172.20.20.6

          PC2:
            kind: linux
            image: ubuntu:latest
            mgmt_ipv4: 172.20.20.7


        links:
          - endpoints: ["R01.TEST:eth1", "SW01.01.TEST:eth1"]
          - endpoints: ["SW01.01.TEST:eth2", "SW02.01.TEST:eth1"]
          - endpoints: ["SW01.01.TEST:eth3", "SW02.02.TEST:eth1"]
          - endpoints: ["SW02.01.TEST:eth2", "PC1:eth1"]
          - endpoints: ["SW02.02.TEST:eth2", "PC2:eth1"]




2. Пинги с крайних устройств





### Вывод
В ходе лабораторной работы с помощью файла конфигурации сети была развернута сеть из роутера, 3-х коммутаторов и 2х ПК и поднят dhcp-сервер в 2-х vlan-ах для автоматической раздачи ip адресов.
