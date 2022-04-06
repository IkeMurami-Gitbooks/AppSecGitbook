# mitmproxy

Установка: pip install mitmproxy или через brew/apt/...\
Документация: [https://docs.mitmproxy.org/stable/tools-mitmdump/](https://docs.mitmproxy.org/stable/tools-mitmdump/)\
Что делать с SSL-трафиком: [https://docs.mitmproxy.org/stable/howto-wireshark-tls/](https://docs.mitmproxy.org/stable/howto-wireshark-tls/)\
Установка серта: [https://docs.mitmproxy.org/stable/concepts-certificates/](https://docs.mitmproxy.org/stable/concepts-certificates/)

Базовые примеры работы:\
Запуск с сохранением ключей шифрования:\
MITMPROXY\_SSLKEYLOGFILE="$PWD/.mitmproxy/sslkeylogfile.txt" mitmproxy\
Прослушивание на указанном порту: mitmproxy -p 44443

mitmproxy - интерактивный инструмент\
mitmdump - аналог tcpdump для http-протокола, а также не интерактивный аналог mitmproxy

Прокидывание трафика:\
mitmproxy -p 9999 --mode upstream:localhost:8888 --ssl-insecure

Можно писать модули (наз wrapper)\
Пример: [https://github.com/trololomgwtf/mitmproxy-wrapper/](https://github.com/trololomgwtf/mitmproxy-wrapper/)
