# mitmproxy

## Install

Python

```
$ pip install mitmproxy
```

Можно через brew/apt/...

Docker

```
$ docker pull mitmproxy/mitmproxy
```

## Running (via Docker)

mitmproxy — интерактивный CLI инструмент

mitmweb — поднимаем mitmproxy на 8080 и веб-интерфейс к нему на 8081:

```
docker run --rm -it -p  8080:8080 -p 127.0.0.1:8081:8081 mitmproxy/mitmproxy mitmweb --web-host 0.0.0.0
```

mitmdump — аналог tcpdump для http-протокола, а также неинтерактивный аналог mitmproxy

```
docker run --rm -it -p 8080:8080 mitmproxy/mitmproxy mitmdump --set ssl_insecure=true
```

## Usage notes

Запуск с сохранением ключей шифрования

```
MITMPROXY_SSLKEYLOGFILE="$PWD/.mitmproxy/sslkeylogfile.txt" mitmproxy
```

Прослушивание на указанном порту

```
mitmproxy -p 44443
```

Прокидывание трафика

```
mitmproxy -p 9999 --mode upstream:localhost:8888 --ssl-insecure
```

mitmproxy сейчас поддерживает множество новшеств, например, wireguard mode. Возможно стоит к нему присмотреться, а не к BurpSuite: [https://mitmproxy.org/posts/releases/mitmproxy9/](https://mitmproxy.org/posts/releases/mitmproxy9/)

## Plugins

Можно писать модули (наз wrapper)\
Пример: [https://github.com/trololomgwtf/mitmproxy-wrapper/](https://github.com/trololomgwtf/mitmproxy-wrapper/)

Есть community модули

## Docs

Документация: [https://docs.mitmproxy.org/stable/tools-mitmdump/](https://docs.mitmproxy.org/stable/tools-mitmdump/)\
Что делать с SSL-трафиком: [https://docs.mitmproxy.org/stable/howto-wireshark-tls/](https://docs.mitmproxy.org/stable/howto-wireshark-tls/)\
Установка серта: [https://docs.mitmproxy.org/stable/concepts-certificates/](https://docs.mitmproxy.org/stable/concepts-certificates/)
