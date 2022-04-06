---
description: CryptoPro, КриптоПро
---

# Web Application Testing with CryptoPro Sign

## Docker

Автор этой полезной заметки: @sorokinpf\
github: [https://github.com/sorokinpf](https://github.com/sorokinpf)

Заметка если вдруг кому то придется тестить сайт на отечественной крипте.

Можно больше не использовать КриптоПро (у неё лицензия и т.д.). Добрый человек собрал openssl с gost engine, а также с этим делом пересобрал stunnel и запихнул все в докер: [https://github.com/gosha20777/openssl-gost](https://github.com/gosha20777/openssl-gost) и на dockerhub: [https://hub.docker.com/repository/docker/gosha20777/openssl-gost](https://hub.docker.com/repository/docker/gosha20777/openssl-gost).

Можно сделать конфигурацию stunnel вот так:&#x20;

```
[configname]
client = yes
accept = 0.0.0.0:6666
connect = yourhost_target:443
```

И Dockerfile:&#x20;

```
FROM gosha20777/openssl-gost:dev

COPY ./stunnel.txt /tmp/stunnel.txt
EXPOSE 6666

ENTRYPOINT stunnel /tmp/stunnel.txt && tail -f /dev/null
```

Далее на 6666 можем заходить по http.

Если есть клиентский серт, то тоже не беда.&#x20;

Конфиг:&#x20;

```
[configname]
client = yes
accept = 0.0.0.0:6666
connect = yourhost_target:443
Cert = /tmp/old_certs/2019 GOST2021.pem
key = /tmp/old_certs/private.key
```

Если закрытый ключ к серту передали в контейнере CryptoPro, то его надо конвертнуть с помощью вот этой хрени: [https://github.com/kulikan/privkey](https://github.com/kulikan/privkey). Там лежит архивчик с экзешником, который работает, если лень разбираться с компиляцией.

```
.\privkey.exe Test1.000 > private.key
```

Внутри директории Test1.000 лежит следующее: &#x20;

![](<../../.gitbook/assets/изображение (29).png>)

Для удобства использования, можно обернуть это Docker-контейнер в nginx: [https://z3f1r.gitbook.io/admin/tools/servers/web/nginx](https://z3f1r.gitbook.io/admin/tools/servers/web/nginx)

Так же, если на стороне клиента происходит проверка подписи, то для корректной работы с ней, нужен браузер, собранный с поддрежкой отечественного шифрования — Chrome Gost — [https://github.com/deemru/chromium-gost/](https://github.com/deemru/chromium-gost/).

## Proxy Settings

Другой вариант:

Настраиваем виндовую виртуалку, там устанавливаем и настраиваем CryptoPro CSP по инструкции: [https://www.cryptopro.ru/products/cades/plugin](https://www.cryptopro.ru/products/cades/plugin)

На этой машине устанавливаем любой прокси, который использует WinAPI (Burp и Gost не подходят), например, Fiddler.

На хостовой машине подрубаем Burp и делаем upstream на виндовую машину. Fiddler уже сам обернет трафик над CryptoPro CSP.

Что делать с контейнером ключа Test2021.000 (взято с [форума](https://www.cryptopro.ru/forum2/default.aspx?g=posts\&t=12820) разработчика):&#x20;

```
Вам нужно выполнить несколько шагов.
1.1) Создать на флешке директорию "abc.000"
1.2) Поместить в неё все key-файлы.
2.1) Панель управления - КриптоПро CSP - Сервис - Скопировать.
2.2) Выбираете контейнер на флешке - выбираете куда копировать (например, Реестр).
3.1) Панель управления - КриптоПро CSP - Сервис - Установить сертификат.
3.2) Выбираете cer-файл и ставите его в новый контейнер.

Вроде, теперь всё должно работать.

Пункт 2 можно заменить на копирование папки на несистемный раздел жесткого диска (не C:). 
```

Сертификат УЦ надо добавить в корневые доверенные сертификаты
