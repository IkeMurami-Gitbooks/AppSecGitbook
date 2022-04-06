---
description: Тулза для митма WiFi/Bluetooth/Ethernet и других протоколов. Написана на Go
---

# Bettercap

## Install

stable: docker pull bettercap/bettercap\
или\
unstable: docker pull bettercap/dev

Чет через докер нихера не запустился (не ддосступен веб с хоста, хотя вроде указано, что должен быть в сети..)

## Usage

```
docker run -it --privileged --net=host bettercap/bettercap -h
bettercap -iface <interface: brifdge100> -eval ''
```

## Commands

Через ; можно объединять команды в одну строчку - `cmd1; cmd2; ...`

### Core commands

```
help - список команд, модулей и статуса - запущен/не запущен
help <module_name>

active - какие модули запущены и их параметры

get PARAMETER значение параметра (длдя получения всех параметров - get *)
set PARAM VALUE - можно использовать "" или ''
read PARAMETER PROMPT - получение параметра из IO

clear - очистить экран

include CAPLET - загрузить и запустить каплет в текущеей сессии

!COMMAND - запустить какую-нибудь команду в shell и вывести результат

alias MAC NAME - создание алиасов для адресов (может не только) (сохраняются в ~/bettercap.aliases)
    алиасы доступны всем модулям

net.show

```

## Modules

В `bettercap` скрипты над командами называются `caplets`.&#x20;

### Web UI

#### Local UI

Чтобы поднять веб интерфейс, нам нужен caplet `http-ui`. Стартуем здесь: `127.0.0.1`

Запускаются `api.rest` и `http.server` модули.

Логин и пароль задать в настройках caplet: `/usr/local/share/bettercap/caplets/http-ui.cap`

Запуск:&#x20;

```
/usr/local/share/bettercap/caplets/http-ui.cap
```

#### Remote UI

Если нам надо поднять для удаленного доступа, то используем caplet https-ui. Настраивается так же. Поднимается интерфейс на 0.0.0.0. Изменить креды: `/usr/local/share/bettercap/caplets/https-ui.cap`.

Запуск:

```
sudo bettercap -caplet https-ui
```
