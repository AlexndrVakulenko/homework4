# homework4
Работа с RAID

Vagrantfile разворачивает box Centos7, добавляет 7 дисков по 100 Mb в vm. 
Т.к Образ не содержит утилиту mdadm включен bridget адаптер для установки утилиты и сопутствующих пакетов:
box.vm.network "public_network", type: "dhcp", adapter: 2 

В секции provision: корректировка репозитория и установка: mdadm smartmontools hdparm gdisk

Создание RAID10 и работа с ним отображена на скриншотах
