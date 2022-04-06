---
description: утилита для анализа WiFi сетей
---

# horst

### Позволяет

* Слушать эфир
* Видеть скрытые сети
* Совмещает функции tcpdump/wireshark
* Есть фильтры
* Можно сохранять все найденное

### Установка

git clone [https://github.com/br101/horst.git](https://github.com/br101/horst.git)\
Скорее всего придется еще установить два пакета:\
`apt-get install libncurses-dev libpcap-dev`\
`make`

Сделаем символьную ссылку, чтобы можно было запускать из любого места утилиту `ln -s /opt/horst/horst /usr/local/bin/horst`

### Использование

Переводим адаптер в режим монитора (мы для сети wlan0 добавим интерфейс а не переведем существующий):\
`iwconfig`\
`iw wlan0 interface add mon0 type monitor`

Запускаем утилиту:\
`horst -i mon0 -o save.txt`
